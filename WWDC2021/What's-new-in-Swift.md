# What's new in Swift

> 🗂WWDC21 | Category : Swift
> 
> 🔗 [https://developer.apple.com/videos/play/wwdc2021/10192/](https://developer.apple.com/videos/play/wwdc2021/10192/)
>  <br />
>  <br />
> 🔖 Join us for an update on Swift. Discover the latest language advancements that make your code easier to read and write. Explore the growing number of APIs available as Swift packages. And we'll introduce you to Swift's async/await syntax, structured concurrency, and actors.

> <br />
<br />  

![/WWDC2021/images/What's-new-in-Swift/Untitled.png](/WWDC2021/images/What's-new-in-Swift/Untitled.png)

![/WWDC2021/images/What's-new-in-Swift/Untitled%201.png](/WWDC2021/images/What's-new-in-Swift/Untitled%201.png)

- Diversity
- Update on Swift packages

### Package collections in Xcode

![/WWDC2021/images/What's-new-in-Swift/Untitled%202.png](/WWDC2021/images/What's-new-in-Swift/Untitled%202.png)

Swift Package Collections 

No longer need to search for packages on the Internet, or copy and paste URLs to add them.

You can now simply browse a collection and add packages from new package search sreen in Xcode.

![/WWDC2021/images/What's-new-in-Swift/Untitled%203.png](/WWDC2021/images/What's-new-in-Swift/Untitled%203.png)

[Package Collections](http://swift.org/blog/package-collections)

<br /> 

### Swift Collections

new open source package of data structures that complements those available in the Swift Standard Library.

- **Deque**

    ![/WWDC2021/images/What's-new-in-Swift/Untitled%204.png](/WWDC2021/images/What's-new-in-Swift/Untitled%204.png)

    - Deque is like an Array, except that is supports efficient insertion and removal at both ends.

- **OrderedSet**

    ![/WWDC2021/images/What's-new-in-Swift/Untitled%205.png](/WWDC2021/images/What's-new-in-Swift/Untitled%205.png)

    - OrderedSet is a powerful hybrid of an Array and a Set.
    - Like Array, OrderedSet maintain its elements in order and supports random access.
    - Like a Set, OrderedSet ensures each element apprears only once and provides efficient membership testing.
- **OrderedDictionary**

    ![/WWDC2021/images/What's-new-in-Swift/Untitled%206.png](/WWDC2021/images/What's-new-in-Swift/Untitled%206.png)

    - OrderedDictionary, which is a userful alternative to Dictionary when order is important, or we need random access to elements.

<br /> 

### The Alogrithms package

![/WWDC2021/images/What's-new-in-Swift/Untitled%207.png](/WWDC2021/images/What's-new-in-Swift/Untitled%207.png)

New open source package of Sequence and Collections algorithms.

![/WWDC2021/images/What's-new-in-Swift/Untitled%208.png](/WWDC2021/images/What's-new-in-Swift/Untitled%208.png)

<br /> 

### Swift System

![/WWDC2021/images/What's-new-in-Swift/Untitled%209.png](/WWDC2021/images/What's-new-in-Swift/Untitled%209.png)

![/WWDC2021/images/What's-new-in-Swift/Untitled%2010.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2010.png)

![/WWDC2021/images/What's-new-in-Swift/Untitled%2011.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2011.png)

<br /> 

### Swift Numerics

![/WWDC2021/images/What's-new-in-Swift/Untitled%2012.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2012.png)

log, sine, cosine

Because these implementations are written in Swift, they are frequently more efficient than a traditional C library and allow for optimizations. 

<br /> 

### The ArgumentParser package

![/WWDC2021/images/What's-new-in-Swift/Untitled%2013.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2013.png)

<br /> 

## Update on Swift on server

 

![/WWDC2021/images/What's-new-in-Swift/Untitled%2014.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2014.png)

**Static linking on Linux**

improves application startup time, as well as simplifies the deployment of server applications, which can now be deployed as a single file.

**Improved JSON performance**

in Swift 5.5, the JSON encoding and decoding used on Linux were reimplemented from scratch, resulting in performance gains for most common use cases.

**Enhanced AWS Lambda runtime**

enhanced and optimized the performance of the AWS Lambda runtime library itself.

All this work made Swift programs running on AWS Lambda start 33% fatser, as well as 40% faster invocation time for a lambda routed via AWS API Gateway.

![/WWDC2021/images/What's-new-in-Swift/Untitled%2015.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2015.png)

<br /> 

## Developer experience improvements

### Swift DocC

![/WWDC2021/images/What's-new-in-Swift/Untitled%2016.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2016.png)

A documentation compiler that's deeply integrated inside Xcode 13, to help you teach developers how th use your Swift framework or package.

 

![/WWDC2021/images/What's-new-in-Swift/Untitled%2017.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2017.png)

In Swift 5.5, we invested in quality and performace improvements in the type checker.

One result of this is you will see fewer "expression too complex" erros when compiling your code.

We also sped up the performance for type checking of array literals.

![/WWDC2021/images/What's-new-in-Swift/Untitled%2018.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2018.png)

<br /> 

### Memory Management

ARC automatically frees up the memory used by class instances when those instances no longer needed.

![/WWDC2021/images/What's-new-in-Swift/Untitled%2019.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2019.png)

To do this, the Swift compiler inserts a retain operation any time a new reference is created and a release operation whenever a new reference stops being used.

New way to track references inside the compiler that allows the compiler to significantly reduce the number of retain and release operations.

![/WWDC2021/images/What's-new-in-Swift/Untitled%2020.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2020.png)

Xcode setting, Optimize Object Lifetimes, that will allow you to see the effect of this new, more aggressive ARC optimization on your code.

<br /> 

## Ergonomic improvements

![/WWDC2021/images/What's-new-in-Swift/Untitled%2021.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2021.png)

### **Result Builders**

- Originally designed for SwiftUI
- Flexible way to define complex object hierarchies
- Standardize and refined this year

<br /> 

### **Enum Codable synthesis**

![/WWDC2021/images/What's-new-in-Swift/Untitled%2022.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2022.png)

You just need to declare the Codable conformance, and the compiler will do all of boilerplates that work for you.

Flexible static member lookup

Type inference in Swift means you can omit redundant type information. 

But Enum-like structures are also represented in other ways. 

![/WWDC2021/images/What's-new-in-Swift/Untitled%2023.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2023.png)

You might have a collection of types that conform to a protocol and want to use instances of those types in your API.

You can now refer to instances of those types using the same dot-notation that you use for Enums, by declaring a few static properties on your protocol.

This is enabled by improvements to Swift's type checker that allows it to reason more generally about static properties in generic contexts, including chained property references such as the .large.

This allows library authors to build sophisticated generic data models with natural and easy-to-use Enum-like APIs.

<br /> 

### **Property wrappers on parameters**

![/WWDC2021/images/What's-new-in-Swift/Untitled%2024.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2024.png)

`@NotEmpty`

With the implementation, those same property wrappers can now be used on function and closure parameters.

<br /> 

## Asynchronous and concurrent programming

 

![/WWDC2021/images/What's-new-in-Swift/Untitled%2025.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2025.png)

Software projects are composed out of blocks of code that execute in some order.
In the simplest case, those blocks execute, on after the other in a simple sequence. But other structures are common as well.

Networking APIs are often designed in an asynchronous style. Int these APIS, after you've sent a request to the remote server, there may be a long delay until you receive a response and need to do more work. Ideally, your code would be suspended during this delay so it does not use any resources until you are able to act on the reponse.

In contrast, concurrent code is when you have two or more blocks of code that you would like to have running at the same time. These are often independent but related operations. Processing several frames of a video, for instance, or running the next iteration of an ML classfier at the same time that you're updating the UI with previous set of results.

<br /> 

### **Asynchronous Programming with Async/Awailt**

![/WWDC2021/images/What's-new-in-Swift/Untitled%2026.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2026.png)

This uses Foundation's URLSession class to make a network call.

The dataTask method is an asynchrounoush operation. You call it with a closure argument. When the result becomes available, your closure will be called with the results to process.

Using closures in this way to express asynchronous code results in a somewhat awkward order of operations.

![/WWDC2021/images/What's-new-in-Swift/Untitled%2027.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2027.png)

`await`

tell the compiler that our function can be suspended as soon as the data method begins and that we won't be able to finish the assignment until that operation has completed.

Syntatically, the async and awail keywords are used similary to throws and try.

`aysnc` decorates the function declaration to indicate that this function must be com mpiled to support suspension.

`await` keyword to mark any call to an async function, method, or closure.

<br /> 

### Structed Concurrency

![/WWDC2021/images/What's-new-in-Swift/Untitled%2028.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2028.png)

`async` 

that it will able to suspend if it needs to wait for results that are being computed in other threads. 

![/WWDC2021/images/What's-new-in-Swift/Untitled%2029.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2029.png)

`async let` 

to run the first two operatipons in parallel.

This initialization will run in parallel with other code out you tryh to use th results.

<br /> 

### Actors

Whenever two seperate threads share data, you run the risk that the data will be inconsistent, or even corrupted.

Swift's new actor construct helps protect your data against such problems.

![/WWDC2021/images/What's-new-in-Swift/Untitled%2030.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2030.png)

If two or more threads call the increment method at the same time, you can end up with a badly-corrupted count.

![/WWDC2021/images/What's-new-in-Swift/Untitled%2031.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2031.png)

actor protects against such corruption

Actors work by suspending any operation that might cause data corruption until it's safe to make that particular change.

![/WWDC2021/images/What's-new-in-Swift/Untitled%2032.png](/WWDC2021/images/What's-new-in-Swift/Untitled%2032.png)

This means you generally need to use await when you call an actor method from outside of the actor.

Actors also work seamlessly with async/await. 

Marking this publish method as async allows it to be suspended while waiting on network operations. While its suspended, other methods can run on this actor without waiting for the network operation to complete and without risk of data corruption.

- Actors are reference types, like clsses, but they obey a number of rules designed to ensure that actors are safe to use in a multi-threaded environment.
- By packaging your data into actors, you are clearly stating that you expect this data to be accessed concurrently and that you want the Swift compiler and runtime to coordinate access so that no corruption is possible.
