# What's new in Swift

> 📅 2020.6.24 (WED)
>
> 🗂WWDC2020 | Session : 10170 | Category : Swift
>
> 🔗 [https://developer.apple.com/videos/play/wwdc2020/10170/](https://developer.apple.com/videos/play/wwdc2020/10170/)
>  <br />
>  <br />
> 🔖 Join us for an update on Swift. Discover the latest advancements in runtime performance, along with improvements to the developer experience that make your code faster to read, edit, and debug. Find out how to take advantage of new language features like multiple trailing closures. Learn about new libraries available in the SDK, and explore the growing number of APIs available as Swift Packages.
> <br />
<br />  

  
**Runtime Performance**

##  Code Size

Code size is the part of the app that represents the machine code representation of the app's logic.

### Internal benchmark: binary size

![/WWDC2020/images/Whats-new-in-Swift/Untitled.png](/WWDC2020/images/Whats-new-in-Swift/Untitled.png)

To track progress, we have been using a Swift rewrite of one of the apps that ships with iOS.  

At each subsequent release we narrowed the difference. With Swift 5.3, we're down to below one and a half times the code size of the Objective-C version.

Different styles of applications, however, produce different binary sizes.

What about a SwiftUI app?

### MovieSwiftUI binary size

![/WWDC2020/images/Whats-new-in-Swift/Untitled%201.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%201.png)

In Swift 5.3, there are significant improvements to the code size of SwiftUI apps. We see that its application login code size is reduced by over 40 percent.

Now, binary size is essential for things like download times. But, when you're running the app, it's part of what we call "clean memory." That's memory that can be purged because it can be reloaded when needed. So it's less critical then "dirty memory", the memory the application allocates and manipulates at runtime.

##  Dirty Memory

### Example of memory layout

> Objective-C

Swift's use of value types has some fundamental advantages over reference-type based languages.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%202.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%202.png)

In Objective-C, object variables are just pointers. So the array holds pointers to the model objects. Those objects, in turn, hold pointers to their properties. Every object you allocate

has some overhead and performance and memory use.

This is so important that Objective-C has a special, small string representation for tiny ASCII strings that allow them to be stored within the pointer, which saves on allocating the extra object.

> Swift

![/WWDC2020/images/Whats-new-in-Swift/Untitled%203.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%203.png)

Swift's use of value types avoids the need for many of these values to be accessed via a pointer.  Thus the UUIDs can be held within the Mountain objects, and Swift's small string can hold many more characters up to 15 code units including non-ASCII characters. 

![/WWDC2020/images/Whats-new-in-Swift/Untitled%204.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%204.png)

Finally, all the Mountain objects can be allocated directly within the array storage. So with the exception of a few strings, everything is held within a contiguous block of memory. Swift programs can get a significant memory benefit from using value types like this.

### Heap usage

> 400 model objects

![/WWDC2020/images/Whats-new-in-Swift/Untitled%205.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%205.png)

So if we examine the heat memory used by an array of 400 of these model objects, we can see the Swift model data is more compact.

> Model and language/library overhead

![/WWDC2020/images/Whats-new-in-Swift/Untitled%206.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%206.png)

some Swift programs previously still use more heat memory because of runtime overhead. Previously, Swift created a number of caches and memory on startup. These caches stored things like protocol conformists and other type information as well as data used to bridge types over to Objective-C.

> Swift 5.3

![/WWDC2020/images/Whats-new-in-Swift/Untitled%207.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%207.png)

All language runtimes have some overhead, but in Swift's case, it was too large. This was a big focus for us to optimize. In Swift 5.3, we've cut that overhead way down to the point where the Swift version of the app now uses less than a third of the heat memory it did in last year's release. 

To get full advantage of these improvements, an app's minimum deployment target needs to be set to iOS 14.

### Lowering the Swift runtime in the userspace stack

![/WWDC2020/images/Whats-new-in-Swift/Untitled%208.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%208.png)

In most applications, these differences are not often noticeable, but optimizing Swift's memory like this was critical to allow us to push Swift further throughout Apple's system, to be able to use it in demons and low level frameworks where every byte of memory used counts.

We have moved Swift's Standard Library, so it sits below Foundation in the stack. That means it can actually be used to implement frameworks that will float below the level of Objective-C where previously C had to be used.

**Developer Experience**

##  Diagnostics

Errors, and warnings from the compiler has vastly improved in this release cycle.

### New diagnostics subsystem in the Swift compiler

![/WWDC2020/images/Whats-new-in-Swift/Untitled%209.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%209.png)

The Swift compiler has a new diagnostic strategy that results in more precise errors that point to the exact location in source code where the problem occurs. There are new heuristics for diagnosing the cause of issue that lead to actionable errors with guidance on how to fix issues.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2010.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2010.png)

Here's an example of an incomprehensible diagnostic that the Swift 5.1 compiler would have produced in SwiftUI code a year ago.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2011.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2011.png)

Fast forward today, the diagnostic is significantly better with an error that tells you exactly what is the problem.

When diagnosing issue, the compiler internally also records more information about problems so it now produces additional notes.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2012.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2012.png)

In this case, applying the fix it, naturally guides developer to provide the missing pieces to an incomplete initialization of a TextField.

Developer Experience

##  Code Completion

All the way from the code completion inference provided by the compiler in SourceKit and trough the experience in the Xcode code editor.

### Improved type-checking inference

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2013.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2013.png)

First, the inference of candidate completions has significantly improved. Here, the compiler is inferring the value in a ternary expression when used within an incomplete dictionary literal.

### Code completion: KeyPath as function

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2014.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2014.png)

Code completion also provides the values you would expect for some of the more dynamic features of the language such as using KeyPath as functions.

### 15x Code completion speed improvement compared to Xcode 11.5

Beside the quality of completion results, code completion performance has drastically improved in some cases up to 15 times speed improvement.

### Code completion performance on SwiftUI code

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2015.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2015.png)

This is particularly beneficial for editing SwiftUI code.

**Developer Experience**

##  Code Indentation

Code indentation in Xcode, also powered by the open-source SourceKit engine, has significantly improved.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2016.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2016.png)

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2017.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2017.png)

Here you can see one of the improvements at work in SwiftUI code taken from the open-source MovieSwiftUI project. Before you would get sometimes a natural indentation for some of he chained accesses.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2018.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2018.png)

But now, they are cleanly visually aligned. These and the other improvements I mentioned will have a noticeable effect on your editing experience.

**Developer Experience**

##  Debugging

### Better error messages for runtime failures

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2019.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2019.png)

When debug information is available, the debugger will now display the reason for common Swift runtime failure traps instead of just showing an opaque invalid instruction crash.

### Increased robustness in debugging

> The pros and cons of using modules for debugging

- LLDB uses Swift Clang modules to resolve types
- LLDB can encounter different modules failures than at compile-time

Swift imports APIs from Objective-C using clang modules. To resolve information about types and variables, LLDB needs to import all Swift and clang modules that are visible in the current debugging context. While these module files have a wealth of information about types, since LLDB has a global view of the entire program and all of its dynamic libraries, importing clang modules that can sometimes fail in ways they would not at compile time.

(ex.  When the search pass from different dynamic libraries are in conflict.)

> 🆕 Using DWARF debug information for Objective-C types in Swift

- LLDB now can resolve Objective-C types in Swift using debug information
- Increases reliability of core debugging features like Xcode variable view

As a fallback, when this occurs, LLDB can now also import C and Objective-C types for Swift debugging purpose from DWARF debug information. This vastly increase the reliability of features such as the Xcode variable view and expression evaluator. 

### Official Swift platform support

- Apple platforms
- Ubuntu 16.04, 18.04, 20.04
- CentOS 8
- Amazon Linux 2
- Windows (coming soon!)

With these efforts comes increased opportunity to use Swift in more places.

Developer Experience

##  Swift on AWS Lambda

Serverless functions are an easy way for client application developers to extent their applications into the cloud.  

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2020.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2020.png)

It is now easy to do this in Swift  using the open-source Swift AWS runtime.

```swift
import AWSLambdaRuntime

Lambda.run { (_, evnet: String, callback) in
	callback(.success("Hello, \(event)"))
}
```

The amount of code needed is as simple as writing hello world.

# Language and Libraires

## Language

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2021.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2021.png)

[Swift Evolution](http://apple.github.io/swift-evolution)

### Multiple trailing closure syntax (SE-0279)

```swift
// Multiple trailing closure syntax

UIView.animate(withDuration: 0.3, animations: {
	self.view.alpha = 0
})

⬇️⬇️⬇️

UIView.animate(withDuration: 0.3) {
	self.view.alpha = 0
}
```

Since its inception, Swift has supported something called trailing closure syntax that lets you pop the final argument to a method out of the parentheses when it's a closure.

It can be more concise and less nested without loss of clarity, making the call site much easier to read.

However, the restriction of trailing closure syntax to only the final closure has limited its applicability.

```swift
UIView.animate(withDuration: 0.3, animations: {
	self.view.alpha = 0
}) { _ in
	self.view.removeFromSuperview()
}
```

In this case the trailing closure makes the code harder to read because its role is unclear. Worse, it changes meaning from the completion block at one call site to the animation block at another.

Concerns about call site confusion have led Swift style guides to prohibit the use of trailing closure syntax when a method call like this one has multiple closure arguments.

```swift
UIView.animate(withDuration: 0.3, animations: {
	self.view.alpha = 0
}, completion: { _ in
	self.view.removeFromSuperview()
})
```

```swift
UIView.animate(withDuration: 0.3) {
	self.view.alpha = 0
}
⬇️
UIView.animate(withDuration: 0.3, animations: {
	self.view.alpha = 0
}) { _ in
	self.view.removeFromSuperview()
}
```

As a result, if we ever need to append an additional closure argument, many of us find ourselves having to rejigger our code more than may seem necessary

🆕 SE-0281

```swift
UIView.animate(withDuration: 0.3) {
	self.view.alpha = 0
}
⬇️
UIView.animate(withDuration: 0.3) {
	self.view.alpha = 0
} completion: { _ in
	self.view.removeFromSuperview()
}
```

New to Swift 5.3 is multiple trailing closure syntax. This extends the benefits of trailing closures syntax to calls with multiple closure arguments and there's no rejiggering required to append an additional one.

Multiple trailing closure syntax is also a great fit for DSLs. SwiftUI's new Gauge view is used to indicate the level of a value relative to some overall capacity. 

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2022.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2022.png)

By taking advantage of multiple trailing closure syntax, Gauge is able to elegantly, progressively disclose is customization points.

### API Design

> 🆕 Trailing closure syntax

```swift
// API design : trailing closure syntax

extension Collection {
	func prefix(while predicate: (Element) -> Bool) -> SubSequence
}

message.body = "Hello WWDC!\n--Kyle"
let summary = message.body.prefix { !$0.isNewline }
assert(summary, "Hello WWDC!"
```

A better name for take might be something like prefix, which suggest the result is anchored to the start of the collection.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2023.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2023.png)

It's important the base name with a method clarify the role of the first trailing closure because its label will be dropped even if it isn't the first argument to the method.

## KeyPath expression as functions (SE-0249)

 

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2024.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2024.png)

Back in Swift 4.1, we introduced smart KeyPaths. Types that represent uninvoked references to properties which can be used to get and set their underlying values.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2025.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2025.png)

⬇️

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2026.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2026.png)

When you're designing and API, KeyPaths are a tempting alternative to function parameters if you expect the call site to be a simple property access because they're more concise and less nested.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2027.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2027.png)

⬇️🆕

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2028.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2028.png)

You can use a KeyPath expression as a function. This means you can pass a KeyPath argument  to any function parameter with a matching signature, and you can delete any duplicate declarations you may have added in the past in order to accept KeyPaths.

## @main (SE-0281)

A tool for type-based program entry points.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2029.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2029.png)

Since Swift 1.0,  you've been able to use the UIApplicationMain attribute on your app delegate to tell the compiler to generate an implicit main.swift that runs your app.

In Swift 5.3, we've generalized and democratized this feature. 

If you're a library author, just declare a static main method on the protocol or superclass you expect your users to derive their entry point from.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2030.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2030.png)

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2031.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2031.png)

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2032.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2032.png)

This will enable your users to tag that type with @main and the compiler to generate an implicit main.swift on their behalf.

This standardized way to delegate a program's entry point, should make it easier to get up and running  whether you're working on a command line tool, an existing application.

## Increased availability of implicit self in closures (SE-0269)

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2033.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2033.png)

In order to dray attention to potential retain cycles, Swift requires the explicit use of `self` in escaping closure's which capture it.

 But when you're required to include many `self` dots in a row, it can start to feel a bit redundant.

In Swift 5.3, if you include `self` in the capture list,  you can omit it from the body of the closure.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2034.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2034.png)

You're still required to be explicit about your intention to capture self. But now the costs of that explicitness is only a single declaration.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2035.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2035.png)

Sometimes, however, even a single use of `self.` can feel unnecessary, like in SwiftUI, where `self` tends to be a value type, making reference cycles much less likely.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2036.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2036.png)

With Swift 5.3, if `self` is a struct or enum, you can omit entirely from the closure.

## Multi-pattern catch clauses (SE-0276)

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2037.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2037.png)

Historically, do catch statements have not been as expressive as switch statements, leading folks to resort to nesting switches inside of catch clauses.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2038.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2038.png)

In Swift 5.3, we've extended the grammar of catch clauses  to have the full power of switch cases. This allows you to flatten the kind of multiclause pattern matching directly into the do catch statement, making it much easier to read.

## Enum Enhancements

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2039.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2039.png)

Since Swift 4.1, the compiler's been able to synthesize equatable and hashable conformance for a wide variety of types.

Sometimes though, you run into situations where it'd be awfully convenient to have a comparison operator.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2040.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2040.png)

In Swift 5.3, the compiler has learned how to synthesize comparable conformance for qualifying enum types.

 **Enum cases as protocol witnesses.**

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2041.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2041.png)

In Swift 5.3, we've enhances enum cases so they can now be used to fulfill static var  and static func protocol requirements.

### Embedded DSL Enhancements

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2042.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2042.png)

This included builder closures to collective use children and basic control flow statements like if else.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2043.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2043.png)

In Swift 5.3, we've extended embedded DSLs to support pattern matching control flow statements like if let and switch.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2044.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2044.png)

I've got a main window for my primary user interface and a preference window for my app settings.

Previously, to use DSL syntax at the top level of body like this, required tagging it with the specific builder attribute.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2045.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2045.png)

In Swift 5.3, the builder attribute will no longer be required because we're teaching the complier how to infer it from the protocol requirement.

> What's new in SwiftUI - WWDC20

## SDK

### Float16

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2046.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2046.png)

Float16 is an IEEE 754 standard floating point format. Float16 takes just 2 bytes of memory as opposed to a single precision float which takes 4.

> Explore Numerical Computing in Swift - WWDC20

### Apple Archive

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2047.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2047.png)

A new modular archive format based on the battle-tested technology Apple uses to deliver OS updates.

idiomatic Swift API

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2048.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2048.png)

file stream constructor, which leverage another new library Swift System.

### Swift System

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2049.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2049.png)

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2050.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2050.png)

The raw, weakly typed interfaces imported though the Darwin overlay can be finicky and error-prone.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2051.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2051.png)

Swift System wraps these APIs using techniques such as strongly typed RawRepresentable structs, error handling, defaulted arguments, namespaces, and function overloading, laying the groundwork for a more idiomatically Swift system layer of the SDK.

### OSLog

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2052.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2052.png)

If you still using `print` as your logging solution, now is the perfect time to reconsider.

> Explore Logging in Swift - WWDC20

## Packages

 

### Swift Numerics

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2053.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2053.png)

Swift Numerics complex numbers are layout compatible with their C counterparts but faster and more accurate.

> Explore Numerical Computing in Swift - WWDC20

### Swift ArgumentParser

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2054.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2054.png)

A New open-source Swift package for command-line argument parsing.

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2055.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2055.png)

### Swift StandardLibraryPreview

![/WWDC2020/images/Whats-new-in-Swift/Untitled%2056.png](/WWDC2020/images/Whats-new-in-Swift/Untitled%2056.png)

Now, you can provide the implementation for standard library feature proposal as a standalone SwiftPM package.
