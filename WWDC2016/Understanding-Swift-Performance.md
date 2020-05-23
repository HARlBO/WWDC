# Understanding Swift Performance

>  📅 2020.5.14 (THU)
>  
>  WWDC2016 | Session : 416 | Category : Performance   

🔗 [Understanding Swift Performance - WWDC 2016 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2016/416/)


![/WWDC2016/images/Understanding-Swift-Performance/Untitled.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled.png)

Swift has a variety of first class types and various mechanisms for code reuse and dynamism.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%201.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%201.png)

Are value or reference semantics more appropriate?

How dynamic do you need this abstraction to be?

Taking performance inplications into account often helps guide me to a more idiomatic solution.

### Understand the implementation to understand performance

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%202.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%202.png)

## Dimensions of Performance

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%203.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%203.png)

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%204.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%204.png)

If we want to write fast Swift code, we're going to need to avoid paying for dynamism and runtime that we're not taking advantage of.

## Allocation

Swift automatically allocates and deallocates memory on your behalf.

### Stack

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%205.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%205.png)

When we call into a function, we can allocate that memory that we need just by trivially decrementing the stack pointer to make space. And when we've finished executing our function, we can trivially deallocate that memory just by incrementing the stack pointer back up to where it was before we called this function.

함수 호출 시에, 스택 포인터가 공간을 만들도록 하면 간단하게 메모리 할당을 할 수 있다.

함수 실행이 끝났을 때도, 스택 포인터를 함수 실행 전에 있던 곳으로 다시 증가시키면 메모리를 해제 할 수 있다.

It's literally the cost of assigning an integer. So this is in contrast to heap, which is more dynamic, but less efficient than the stack.

integer 할당하는 것만큼의 비용이 든다. (빠르다) 더 동적이지만 효율은 안좋은 힙과 비교 대조되는 점이다.

### Heap

The heap lets you do things the stack can't like allocate memory with a dynamic lifetime. 

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%206.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%206.png)

Because multiple thread can be allocating memory on the heap at the same time, the heap needs to protect its integrity using locking or other synchronization mechanisms. This is a pretty large cost.

**Struct**

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%207.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%207.png)

Before we even begin executing any code, we've allocated space on the stack for our point1 instance and our point2 instance. Because point is a struct, the x and y properties are stored in line on the stack.

코드를 실행하기 전부터 이미 point1, point2 인스턴스가 있는 스택이 할당 된다. `Point` 가 struct 이기 때문에, `x` 와 `y` 는 스택에 저장 되어 있다.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%208.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%208.png)

When we go to construct our point with an `x` of 0 and a `y` of 0, all we're doing is initializing that memory we've already allocated on the stack.

point x, y 에 0, 0을 구성할 때 이미 스택에 할당되어 있는 메모리를 초기화 해주기만 하면 된다.

When we assign `point1` to `point2` , we're just making a copy of that point and initializing the `point2` memory, again, that we'd already allocated on the stack.

`point1`을 `point2` 에 할당할 때, 그 point를 복사하고 `point2` 메모리를 초기화 하면 된다.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%209.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%209.png)

Note that `point1` and `point2` are independent instances.

`point1` 과 `point2` 는 독립된 인스턴스이다. `point2.x` 에 5를 할당해도 `point1.x` 의 값은 여전히 0이다.

This is known as value semantics.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2010.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2010.png)

use `point1`, use `point2` and we're done executing our function. So we can trivially deallocate that memory for `point1` and `point2` just by incrementing that stack pointer back up to  where we were when we entered our function.

**Class**

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2011.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2011.png)

Instead of for the actual storage of the properties on point, we're going to allocate memory for references to `point1` and `point2`. References to memory we're going to be allocated on the heap.

heap 에 할당할 때, Swift 는 실제로 4word의 저장 공간을 할당 해준다. 이 점이 2word를 할당 했던 struct 와는 대조적이다.

point 가 class 이기 때문에, point 의 content를 복사 하지 않고 reference를 복사한다.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2012.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2012.png)

This is known as reference semantics and can lead to unintended sharing of state.

We saw that classes are more expensive to construct than structs because classes require a heap allocation. Because classes are allocated on the heap and have reference semantics, classes have some powerful characteristics like identity and indirect storage.

이 경우가 아니라면 Struct를 사용하는게 좋다. 

And stucts aren't prone to the unintended sharing of state like classes are. 

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2013.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2013.png)

실행 될 때, 유저가 스크롤 할 때 마다 자주 호출 되기 때문에 빨라야 한다.

**→ 캐싱 레이어 추가**

(한 번 생성 된 이미지는 저장 해놓고 더이상 생성 하지 않도록)

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2014.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2014.png)

`**String` 은 키 값으로 strong type 이 아니다.** 

- 이미지를 대표 하는 key이긴 하지만 그냥 댕댕이 라고 쉽게 키를 넣을 수도 있다. → Safety 하지 않다.
- character들의 contents 를 간접적으로 heap 에 저장 한다. → `makeBalloon` 함수를 호출 할 때마다, 캐싱을 하더라도 heap allocation 을 하게 된다.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2015.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2015.png)

`**Struct` 를 사용하는 것이 더 safe하다.**

- `Struct`는 Swift에서 first class type이기 때문에 dictionary의 key로 사용 할 수 있다.
- 더이상 heap allocation 을 요구하지 않고 stack에 할당한다.
- 더 안전하고 더 빠르다!

## Refrence Counting

Swift 는 heap에 할당 된 메모리를 언제 해제해야 되는지 어떻게 알까?

⇒ Swift는 reference의 전체 count 수를 heap에 가지고 있다. count 가 0 이 되면 해당 인스턴스를 아무도 가르키고 있지 않다고 판단하고, 메모리를 해제하기 안전하다고 판단한다.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2016.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2016.png)

- multiple thread에서 count 증가, 감소가 일어나기 때문에 thread safety 해야 한다.
- Reference counting 은 굉장히 자주 일어나는 연산이다. → cost can add up.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2017.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2017.png)

Swift 가 `retain`, `release` 를 추가해준다. 

- `retain` : atomically increment reference count
- `release` : decrement reference count

**Class**

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2018.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2018.png)

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2019.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2019.png)

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2020.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2020.png)

**Struct**

Struct 일 때는 reference counting 이 일어나지 않는다.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2021.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2021.png)

**Struct containing references**

`String` 는 character들을 heap에 저장하기 때문에 reference counting이 된다.

`UIFont` 는 class 이기 때문에 reference counting이 된다.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2022.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2022.png)

`label` 은 2개의 reference를 가지고 있다. 

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2023.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2023.png)

copy를 하면 reference가 2개 더 추가 된다.   

**Class는 heap에 할당 되기 때문에 Swift는 heap allocation의 lifetime을 reference counting을 통해 관리 한다.**   

Struct가 reference를 포함 하고 있다면,  **reference counting 을 한다. ⇒ reference가 많을 수록 reference counting overhead 가 생긴다** 

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2024.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2024.png)

🆕value type UUID 

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2025.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2025.png)

More type safety, got more performance, way more convenient to write, way more type safe

## Method Dispatch

### Static

메소드를 runtime에 호출하면, Swift correct implementation을 실행해야 한다.

컴파일 타임에 실행할 implementation을 결정 할 수 있는 것이 `static dispatch` 이다.

런타임에 correct implentation에 직접 jump 할 수 있을 것이다.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2026.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2026.png)

**inlining**

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2027.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2027.png)

complier knows which implementations are going to be executed

```swift
drawAPoint(point)
⬇️
param.draw() ⇒ // static dispatch
⬇️
Point.draw implementation

// no call stack overhead
```

### Dynamic

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2028.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2028.png)

Dynamic dispath 는 컴파일 타임에 결정 할 수 없다. 런타임에 implementation을 찾고 jump 할 수 있다.

그래서 dynamic dispatch 는 static보다 비용이 비싸지 않다. There's just one level of indirection.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2029.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2029.png)

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2030.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2030.png)

We can create an array of these things and they're all the same size because we're storing them by reference in the array

Because this `d.draw()` , it could be a point, it could be a line.

The complier adds another filed to classes which is a pointer to the type information of that class and it's stored in static memory.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2031.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2031.png)

And so when we go and call draw, what the compiler actually generates on our behalf is a lookup through the type to something called the virtual method table on the type and static memory, which contains a pointer to the correct implementation to execute.

**Final Class**

If you never intend for a class to be subclassed, you can mark it as `final` 

## Protocol Types

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2032.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2032.png)

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2033.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2033.png)

struct  `Line`과 `Point` 는  V-table dispatch 를 위해 필요한 상속 관계를 공유하고 있지 않다.

How does Swift dispatch to the correct method?

→ Table based mechanism called the Protocol Witness Table (PWT)

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2034.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2034.png)

타입마다 프로토콜을 구현한 하나의 테이블이 있다. 테이블의 엔트리가 타입의 구현을 링크한다.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2035.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2035.png)

`Line`은 4 words 가, `Point` 는 2 words 가 필요 → 같은 사이즈가 아닌데 array는 fixed offset 

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2036.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2036.png)

`**Point` (2 words)**

The first 3 words  in that existential container are reserved for the valueBuffer.

Small types like our `Point`, which only needs two words, fit into this valueBuffer.

`**Line` (4 words)**

Swift allocates memory on the heap and stores the value there and stores a pointer to that memory in existential container

### The Value Witness Table (VWT)

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2037.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2037.png)

1. Allocate memory on the heap and store a pointer to that memory inside of the valueBuffer of the existential container
2. Swift needs to copy the value from the source of the assignment that initializes our local variable into existential container
3. Copy entries of our value witness table will do the correct thing and copy it into the valueBuffer allocated in the heap
4. Program continues and we are at the end of the lifetime of our local variables
5. Swift calls the deallocate function in that table

### The Existential Container

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2038.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2038.png)

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2039.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2039.png)

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2040.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2040.png)

→ This work is what enables combining value types such as struct `Line` and struct `Point` together with protocols to get dynamic behavior, dynamic polymorphism.

### Protocol Type Stored Properties

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2041.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2041.png)

- Existential Container inline
- Large values on the heap
- Supports dynamic polymorphism

→ This representation allows storing a differently typed value later in the program.

### Expensive Copies of Large Values

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2042.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2042.png)

heap 은 expensive 한데 ? 4 개 heap 을 사용?

→ existential container has place for 3 words and references would fit into those 2 words

### References Fit in the Value Buffer

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2043.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2043.png)

first를 복사해서 second를 만들면 reference count 만 증가 시키면 되지만 unintended sharing이 일어나잖아? 

→ Copy on write

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2044.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2044.png)

### Indirect Storage with Copy-on-Write

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2045.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2045.png)

When we come to modify, mutate our value, we first check the reference count. Is it greater than 1 ?

 If the reference count is greater than 1, we create a copy of our Line storage and mutate that.

### Copy Using Indirect Storage

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2046.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2046.png)

This is a lot cheaper than heap allocation.

### Summary - Protocol Types

- Dynamic polymorphism
- Indirection through Witness Tables and Existential Container
- Copying of large value causes heap allocation

## Generic code

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2047.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2047.png)

Swift will bind the generic type T to the type used at this call side.

Generic parameter T in this call context is bound through the type Point.

### Implementation of Generic Methods

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2048.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2048.png)

Because we have one type per call context, Swift does not use an existential container here. Instead, it can pass both the value witness table of the type used at this call-site as additional arguments to the function.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2049.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2049.png)

And then during execution o that function, when we create a local variable for the parameter, Swift will use the value witness table to allocate potentially any necessary buffers on the heap and execute the copy from the source of the assignment to the destination.

### Storage of Local Variables

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2050.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2050.png)

**Is this any faster? Is this any better?**

This static form of polymorphism enables the compiler optimization called specialization of generics.

### Specialization of Generics

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2051.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2051.png)

### When Does Specialization Happen?

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2052.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2052.png)

### Whole Module Optimization

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2053.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2053.png)

If we compile those 2 files separately, when I come to compile the file `UsePoint`, the definition of my `Point` is no loner available because the compiler has compiled those 2 files separately.

However, with whole module optimization, the compiler will compile both files together as one unit and will have insight into the definition of the `Point` file and optimization can take place.

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2054.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2054.png)

### Specialized Generics - Class Type

**Performance characteristics like class type**

- Heap allocation on creating an instance
- Reference counting
- Dynamic method dispatch through V-Table

![/WWDC2016/images/Understanding-Swift-Performance/Untitled%2055.png](/WWDC2016/images/Understanding-Swift-Performance/Untitled%2055.png)
