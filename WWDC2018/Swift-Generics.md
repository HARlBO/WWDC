# Swift Generics (Expanded)

> 📅 2020.5.26 (TUE)
> 🗂WWDC2018 | Session : 406 | Category : Swift
> 🔗 https://developer.apple.com/videos/play/wwdc2018/406/
>
> 🔖 Generics are one of the most powerful features of Swift, enabling you to write flexible, reusable components while maintaining static type information. Learn about the design of Swift's generics, including how to generalize protocols, leverage protocol inheritance to express the varying capabilities of related types, build composable generic components with conditional conformances, and reason about the interaction between class inheritance and generics. This expanded version of the WWDC 2018 session includes a brand-new discussion of recursive constraints.



![image-20200526063021212](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200526063021212.png)





## What are generics?

### Why Generics?

#### A Simple Buffer Type

```swift
struct Buffer {
  	var count: Int
  
  	subscription(at: Int) -> Any {
      	// get/set from storage
    }
}
```

→ If you wnated to handle anything in the buffer, you could have subscript an `Any`. 



#### Untyped Storage

```swift
var words: Buffer = ["subtyping", "ftw"]

// I know this array contains strings
word[0] as! String

// Uh-oh, now it doesn't!
words[0] = 42
```

→ What if you put an integer into what was supposed to be a buffer of strings?



#### In-Memory Representation

```swift
let numbers: Buffer = [1,1,2,3,5,8,13]
```

Buffer doesn't know in advance what kind of type it's going to contain. So it has to use a type like `Any` that can account for any of the possibilities.
→ There's a lot of overhead in tracking, boxing and unboxing the types in that `Any`

#### Any and Indirection

![image-20200526064406810](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200526064406810.png)

- I might have just wanted a buffer of integers, but I have no way of expressing that to the compiler.

- `Any` has to account for differenct king of type. Including types that are too large to fit inside its own internal storage, it has to sometimes use indirection.

- It has to hold a pointer to the values, and that value could e locataed all over memory.



### Parametric Polymorphism ( = Generics )

```swift
// Generic Buffer Type
struct Buffer<Element> {
  let count: Int
  
  subscript(at: Int) -> Element {
    // fetch from storage
  }
}
```

With a generic approach, we put more information on the buffer, to represent the types that the buffer is going to contain.

![image-20200526065259302](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200526065259302.png)

This means that there's no need to do conversions when you're getting a type out of the buffer.

If you make an accidental assignment of the wrong kind of type, the compiler will catch you.

There's no such type as buffer without an associated element type.



#### Implied Generic Parameters

```swift
let words: Buffer = ["generics", "ftw"]
let words: Buffer<String> = ["generics", "ftw"]

```

This knowledge of exactly what a type like buffer contains is carried all the way through both compile and runtime.  →  We can achieve our goal of holding all of the elements in a contiguous block of memory, with no overhead.



#### Swift's In-Memory Representation

```swift
let numbers: Buffer = [1,1,2,3,5,8,13]

var total = 0
for i in 0..<numbers.count {
  	total += numbers[i]
}

```

The complier has direct knwledge at all times of exactly what element type the buffer contains.



```swift
// Wrap Common Algorithms in Methods

extension Buffer {
  func sum() -> Element {
  	var total = 0
  	for i in 0..<self.count {
      	total += self[i]
    }
    return total
  }
}

let total = numbers.sum()

```

To sum up a buffer of integers, it might make sense to extract it out into a method. An extension on buffer that's more unit-testable, and more readable when you actually call it.

But not all element types can be summed up. We need to tell the compiler more about capabilities the element needs to have.

```swift
// Contain Common Algorithms in Methods

extension Buffer where Element: Numeric {
  func sum() -> Element {
  	var total = 0
  	for i in 0..<self.count {
      	total += self[i]
    }
    return total
  }
}

let total = numbers.sum()

```



## Designing a Protocol

![image-20200526070922476](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200526070922476.png)

It's important to think to start with some concrete types, and then try and unify them with a protocol.

There's a natural push and pull here, between conforming types on the one hand. That want as much flexibility as possible in fulfilling that contract.

![image-20200526071309960](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200526071309960.png)

Collection Protocol

1. We need to represent the element type
2. In protocol, we use an associated type for that

Each conforming type needs to set element to be something appropriate.

`Buffer` , `Array` as of Swift 4.2 
→ This happens automatically because we also named their generic parameters to be element as well.

This is nice side benefic of giving your generic arguments meaningful names (<T> 🙅🏻‍♀️)that follow common conventions like the word element.

`Dictionary`
→  Needs to set element type to be the pair of its key and value type



![image-20200526071421347](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200526071421347.png)

**Adding the subscription operation**

Every conforming type would have to supply the ability to fecth an element's given position that was represented by an integer. And that works great for types like `Array` .

But is it flexible enough for a slightly more complicated rype, like a `Dictionary`?

![image-20200528002904148](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200528002904148.png)

- `Index`
   That has specific logic for moving from one element to the next
  (For example, it could be backed by an internal buffer of some kind, and it could use an index type that stroed an offset into that buffer)
- `subscript` 
  It could then take as the argument to subscript in order to fetch an element to the position using that offset
- But it would be critical that the dictionay's index type be an opaque type that only the dictionary can control.
- `index`
  We want the dictionary to control moving forward through the collection by advancing the index.
- `startIndex` , `endIndex`
  A simple count isn't going to work anymore in order to tell us that we've reached the end.

⬇️

![image-20200528003559697](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200528003559697.png)



**Constraining Index**

`where Index: Equatable`

![image-20200528003758330](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200528003758330.png)

We want protocol to be easy to use. It's probably better expressed as a requirement of the protocol. As a constraint on ur index-assosiate type.

![image-20200528003933559](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200528003933559.png)

- Putting this constraint on the protocol means that all types that conform to the protocol need to supply an equatable type for their index.

- 🆕 New automatic synthesis of equatable conformance.



## Protocol inheritance

#### The Collection Protocol Is Not Enough

Some collection algorithms need more than `Collection` provides:

- `lastIndex(where:)` needs to walk backward to be efficient
- `shuffle()` needs to swap elements to worak at all

Some conforming types have these capabilities



#### Protocol Inheritance: BidirectionalCollection

![image-20200530222134546](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530222134546.png)

What that means is that any type that conforms to the bidirectionCollection protocols also conforms to collection, and you can use those collection algorithms.

Not every collection can actually implement this particular requirement.
→ SinglyLinkedList, where you only have these pointer hopping form one location to the next.



#### Use Inheritance to Provide More-Specialized 

![image-20200530223121980](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530223121980.png)



#### Fisher-Yates Shuffle

![image-20200530223642534](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530223642534.png)

1. Start with an index to the fist in the collection 
2. Select randomly some other element in the collection
3. Swap those two
4. Move the left index forward one
5. Randomly select between there 
6. Swap those elements

It's just this linear march through the collection, randomly selecting another element to swap with.

![image-20200530223928413](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530223928413.png)



#### Keep Distinct Capabilies Separate

- `shuffle()` is using random access and element mutation
- `UnsafeBufferPointer` provides random access without element mutation
- `SinglyLinkedList` provides element mutation but not random access

```swift
protocol RandomAccessCollection: BidirectionalCollection {
  func index(_ position: Index, offsetBy n: Int) -> Index
  func distance(from start: Index, to end: Index) -> Int
}

protocol MutableCollection: Collection {
  subscript(index: Index) -> Element { get set }
  mutating func swapAt(_: Index, _: Index) {}
}
```



#### Clients Can Compose Multiple Protocols

![image-20200530224438558](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530224438558.png)



#### Collection Protocol Hierarchy

![image-20200530224400019](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530224400019.png)

These hierarchies, they shouldn't be too big. They should not be too fine-grained.
Because you really want a small number of protocols that really descibe the kinds of types that show up in. the domain.



## Conditional conformance

##### Slicing Collections![image-20200530224837157](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530224837157.png)

Slice is a generic adaptor type. So it is parameterized on a base collection type, and it tis itself a collection.

But even if the buffer is a bidirectionalCollection, nothing has said that the slice is a bidirectionalCollection. We can fix that.



#### Conditional Conformance to BidirectionalCollection

- Conformance depends on additional requirements

```swift
extension Slice: BidirectionalCollection where Base: BidirectionCollection {
  func index(before ids: Index -> Index { return base.index(before: idx)}
}
  
extension Slice: RandomAccessCollection where Base: RandomAccessCollection {
	func index(_ idx: Index, offsetBy n: Int) -> Index { return base.index(idx, offsetBy: n) }
	func distance(from s: Index, to e: Index) -> Int { return base.distance(from: s, to: e)  } 
}  
```

- Write an extension, have it conforms to one protocol, so you know what this extension if for, you know its meaning.

- It's particulary important with conditional requirements, conformances, because you have differenct requirements on these extensions. -> This allows for composability ( Whatever the underlying base collection can do, the slice type can also do.)



#### Ranges

![image-20200530225542834](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530225542834.png)

It's generic over the underlying bound type. So in this case, we have a range of doubles, and it merely stores the lower and upper bonds.

Prior to Swift 4.2, you would get from an integer range, different type. This is `CountableRange` type. 



#### 🆕 Range as a Collection

##### Range conditionally conforms to `RandomAccessCollection`

```swift
extension Range: Collection, BidirectionalCollection, RandomAccessollection 
	where Bound: Strideable, Bound.Stride: SingedInteger {
    // ...
  }
```

##### `CountableRange` is a convenient alias for Ranges that are `Collections`

```swift
typealias CountableRange<Bound: Strideable> = Range<Bound>
	where Bound.Stride: SignedInteger
```



## Recursive Constraints

```swift
protocol Collection {
  // ...
  associatedtype SubSequence: Collection
}
```

`Collection` has an `associatedtype` named `SubSequence`. That is itself a collection. Why would you need this?



### Insertion into a Sorted Collection

Where should we insert a new value in an already-sorted array?

```swift
var array = [0, 2, 4, 6, 8, 10, 12, 14, 16]
array.insert(11, at: array.sortedInsertionPoint(of: 11))
print(array) // [0, 2, 4, 6, 8, 10, 11, 12, 14, 16]
```



### Divide-and-Conquer Binary Search

Classic divide-and-conquer algorithm. Meaning that at each step, it  makes a decision that allows it to significantly reduce the problem size.

1. Look at the middle element
2. Compare it against the value that we want to insert
3. (If we want to insert is big)Restrict search space by half
4. Find the new the new middle
5. Compare it against the value that we want to insert

The divide-and-conquer algorithms are extremly fast. Binary search takes logarithmic time which means that doubling your input size doesn't make the algorithm run twice as slowly like it would for a linear algorithm.



![image-20200530231743077](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530231743077.png)



### Adding Slicing to Collection

```swift
extension Collection {
  subscript (bounds: Range<Index>) -> Slice<Self> {
    return Slice(base: self, bounds: bounds)
  }
}
```



### Custom Slice Types

```swift
extension String: Collection {
  subscript (bounds: Range<Index>) -> Substring { ... }
}
```

```swift
protocol Range: Collection {
  // ...
  associatedtype SubSequence = Slice<Self>
  subscript (range: Range<Index>) -> SubSequence { get }
}
```

It's slicing operation reruns an instance of the exact same range type jus with the new bounds.



### What Capabilities Do Algorithms Depend on?

![image-20200530232822503](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530232822503.png)

Our algorithm is recursive. If forms a slice which is now a value of the subsequence type. And then recursively calls sort insertion point of on that slice.
This only makes sense if the subsequence type you get back is itself a collection.



### Recursive Constraints

```swift
protocol Collection {
  associatedtype Element
  associatedtype Index
  associatedtype SubSequence: Collection
  
  subscript (range: Range<Index>) -> SubSequence { get }
}
```

 Associatedtype type coforms to its own enclosing protocol.



### Can You Slice a SubSequence?

![image-20200530233341584](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530233341584.png)

Every SubSequences are collection and every collection has a slice operation so of course you can slice a subsequence and the result is going to be a subsequence to the subsequence.

It's often the case divide-and-conquer algorithms can be implemented more efficiently by making them nonrecursive.

![image-20200530233448460](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530233448460.png)

The core algorithm is the same, but it's expressed iteratively with this while loop rather than recursively.

However, here we have a problem. We're performing an assignment to the slice variable which is of the subsequence type.

![image-20200530233852636](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530233852636.png)

Non-Recursive Divide-and-Conquer

![image-20200530233911588](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530233911588.png)

##### 

### Tracking Protocol Inheritane

##### Protocols can strengthen requirements with `where` clauses

```swift
protocol BidirectionalCollection: Collection 
	where SubSequence: BidirectionalCollection {
    // ...
  }
```

##### Recursive requiremnts can "stack"

```swift
protocol RandomAccessCollection: BidirectionalCollection 
	where SubSequence: RandomAccessCollection {
    // ...
  }
```



### Recursive Constraints and Conditional Conformance

![image-20200530233925747](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530233925747.png)

Recursive restraints are a powerful tool. Used with associated type and protocol where clauses, they help us wirte the protocol requirements we need to express divide-and-conquer algorithms naturally in generic code.



## Classes and generics

### Class Inheritance

![image-20200530234510917](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530234510917.png)



### Liskov Substitution Principle

![image-20200530234535187](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530234535187.png)



### Protocol Conformances and Classes

- Protocol conformances are inherited by subclasses

- A signle conformance must work for all subclasses 

  ```swift
  extension Vehicle: Drivable { }
  
  extension Drivable {
    func sundayDrive() {
      if Date().isSunday { drive() }
    }
  }
  
  PoliceCar().sundayDrive()
  ```

  

##### Initializer Requirements

```swift
protocol Decodable {
  init(from decoder: Decoder) throws
}

extension Decodeable {
  static func decode(from decoder: Decoder) throws -> Self {
    return try self.init(from: decoder)
  }
}

class Vehicle: Decodable {
  init(from decoder: Decoder) throws { ... }
}

Texi.decodes(from: decoder)


```

- It returns `Self` . It's the same type that you're calling the static method on.
- We're calling to that initializer above to create a brand-new instance of whatever decodable type we have, and then return it.

### Which Initalizer Gets Called?

![image-20200530235228691](/Users/jinhapark/Library/Application Support/typora-user-images/image-20200530235228691.png)

`required` initializer has to be implementd in al subcalsses.



### Final Classes Have No Subclasses

- `final` classes are exempt from these rules
- Use `final` when your class is no customizable through inheritance

```sw
final class EndOfTheLine: Decodable {
	init(from decoder: Decoder) { ... }
}
```



## Summary

Swift's generics provide code reuse while maintaining static type informaton 

Let the push-pull between generic algorithms and confroming types guide design

- Porotocol inheritance captures specialized capabilities of some conforming types
- Conditional conformance provides composition for those capabilities

Apply the Liskov Substitution Priciple when working with classes