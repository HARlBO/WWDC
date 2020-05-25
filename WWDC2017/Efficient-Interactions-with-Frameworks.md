# Efficient Interactions with Frameworks

>  📅 2020.5.25 (MON)   
>    
> WWDC2017 | Session : 244 | Category : App Frameworks   

   
🔗 [Efficient Interactions with Frameworks - WWDC 2017 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2017/244)
   
     
![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled.png)

There are multiple dimensions, how can we visualize this a little bit better?

### Ⅰ

- If you're looking at a lot of data and code that runs very frequently.
- These are going to be things that are going to be likely to be able to make a notable impact upon performance.
- You're going to want to spend time optimizing these cases.

### Ⅲ

- If you're looking at something that works just a little bit of data and runs just a few times.
- You don't really want to spend a whole lot of time worrying about them too much.

### Ⅱ, Ⅳ

- That are highly dependent upon the situation.
- You'll most certainly want to be able to measure the performance in a scenario that reflects actual usage.
- Use that information to be able to evaluate whether it's worth your time to be able to make changes.

## Improvements in Foundation

**NSCalendar**

- Date enumeration, not only to use less memory, but also, it's much faster too.
- Implementation is not only faster, but also, corrects some outstanding Edge case that have been lurking around for a while.

**Internal locking improvements**

- Migrate to using Atomics and OS and Fairlock, which in the end, ends up playing a lot better with quality of service.

**NSOperation and NSOperationQueue**

- More correct implementation, whenever it comes to their quality of service.
- Neat performance improvements, 25% improvement on queueing operation.

**Copy on write collections**

- A number of collection types will now use copy on write as their backing storage.

### Copy on Write Collections

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%201.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%201.png)

COW is a mechanism, where two items can point to a shared backing store until a mutation occurs. And when that mutation happens, the mutation party copies that backing storage to be albe to allow for the write to happen. 

→ **Copying isn't costly, anymore.**

This means that whenever you defensively copy a mutable container, it's almost free

- before : copies of collections were at best, linear execution time.
- now : whenever you copy them, they're constant until a shared mutation.

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%202.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%202.png)

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%203.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%203.png)

- Sine copies are nearly free, we can do the same thing every single time and not worry about the performance hit.
- You can defensively copy return values so that it does the right thing without having worry about the performance costs.

### Data

> The best types for dealing with bytes

**Data is its own slice**

**Indexing is only a few instructions in optimized builds**

**Appending is dramatically faster**

**Replacing region is faster too**

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%204.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%204.png)

→ Since data is a common currency of dealing with a collection of bytes, this should be really fast.

And if you were using Data, you get this advantage for free. And it will also be able to interoperate with all the rest of the APIs that take and use data.

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%205.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%205.png)

## Bridges and how the affect your app

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%206.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%206.png)

**Toll-free bridging**

- Zero cost at the cast
- The `NSArray` being bridged to a `CFArray`, it's just a reinterpretation of a pointer.
- There is a slight cost to be paid, whenever you pass hat object to `CFArrayGetCount`

**Swift bridges**

- Cost in these particular cases are paid in advance.
- These are then normal costs at the usage.

## Strings, ranges, and text

**Invest in performance that matters to your users**

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%207.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%207.png)

### String bridging

> Example 1 : `UILabel`

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%208.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%208.png)

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%209.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%209.png)

The `NSString` form the framework is a reference type, while Swift's `String` is value type.

And so, when ask the framework for that `NSString`, it's wrapped in the value type when it crosses the Swift bridge.

To preserve Swift value semantics, the framework has to make a copy of it.

In this case, the original `NSString` is immutable, so when the framework makes that copy, it's optimized to just retain, which is pretty cheap. It's just incrementing the ref count. 

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2010.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2010.png)

In this case, the original string consisted of seven ASCII characters. So, even if we make a full copy the impact would be pretty small. 

Most of the time `UILables` are going to consist of short strings that are used for UI display purpose, so you're probably not going to be fetching their text very frequently. → Ⅲ

> Example 2 : `NSTextStorage`

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2011.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2011.png)

Since `NSTextStorage` is intended for working with text editing, it's reasonable to expect the contents of that `textStorage` to be mutated frequently.

And contents of the `textStorage` could also be a very long string. 

So for efficiency , the framework only keeps the mutable string around. So, when you ask for that string property on the text storage, what you'll get is going to be backed by an `NSString` that refers to the mutable string.

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2012.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2012.png)

Just as before, it will be wrapped in the value type when it crosses the bridge, because it's an `NSString`

And the framework is going to make a copy. But `NSString` is mutable, so this copy could be expensive.

And `textStorage` is much more likely to contain long length strings.

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2013.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2013.png)

`NSMutableString` is a reference type that is not bridged. There is no copy. So we avoid cost of copy.

This situation results from a mismatch between Swift's value semantics and the design of `NSTextStorage`, which needs to use reference semantics for performant management of large amounts of text. 

If you working with large amounts of text and the text storage, use `mutableString` to access it even if you don't plan to mutating it.

**How much text do you expect that storage to contain?**

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2014.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2014.png)

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2015.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2015.png)

→ If you use the `string` property, that's probably fine.

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2016.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2016.png)

→ Consider using `mutableString`.

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2017.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2017.png)

→ Really hope you're using `mutableString`

### Ranges

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2018.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2018.png)

> Example 1 : Working with `NSAttributedString`

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2019.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2019.png)

🆕⬇️

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2020.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2020.png)

 

> Example 2 : Working with `NSRegularExpression`

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2021.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2021.png)

We want to print out all the start tags. In order to do this, we'll use NSRegularExpression to find the tags we went, and then append them to a string.

But the `NSRegularExpression` API gives me `NSRange` back from my match groups.

And I need ranges of string index to be able to append to my Swift string.

Convert from `NSRange` to range a string index.

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2022.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2022.png)

🆕⬇️

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2023.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2023.png)

### Text layout and rendering

Text poses some real performance challenges, because of its ubiquity in scale.

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2024.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2024.png)

Factors that our frameworks take into account when we perform text layout and rendering.

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2025.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2025.png)

We look all of these things and more to render your text in a way that's both correct and performant.

→ So, I encourage you to use the standard label controls whenever possible.

> Example 1 :  A Tale of Two Labels

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2026.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2026.png)

She set up her labels with attributed strings and off she went but noticed that the scrolling performance in her app was a little bit slow. So she rendered each line in a separate label, her app scrolling performance improved.

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2027.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2027.png)

But when she tested her app with Chinese text, she discover that the scrolling performance was even slower than before.

![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2028.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2028.png)

**Postmortem**

- Initial conditions qualified for fast rendering path within the framework
The optimization of splitting each line into its own label took advantage of the fact that attributed strings containing only one style of text may qualify for faster rendering. But this isn't sufficient condition for faster rendering.
- Input changes forced rendering to slower path
Splitting the two line strings into separate labels, meant that the app was rendering twice as many labels as it needed to.
- App used older layout practices by manually setting the frames, instead of using auto layout

**What You can do**

**🛠 Higher-level strategy**

- Use standard label controls
    - The framework is in a better place to apply these optimizations, because it has a bigger picture view of the situation and more information about the rendering conditions
    - And when we make performance improvements, you'll automatically get those benefits.
    3x faster rendering with `NSTextField` in mackOS 10.13
- Use modern layout practices like auto layout
    - Text layout and rendering performance with modern practices is very heavily scrutinized on our end.
    - You'll be less likely to run into edge case performance scenarios that we haven't already seen and improved

**🛠 Lower-level tips**

- Set rendering attributes for attributed strings

    ```swift
    let attributes: [NSAttributedStringKey: Any] = [
    								.font: UIFont.systemFont(ofSize: systemFontSize),
    								.paragraphStype: NSParagraphStyle.default,
    								.foregroundColor: UIColor.darkText]

    let myString = NSStringAttributedString(string: "Hello", attributes: attributes)
    ```

    - If you don't supply these attributes yourself, the text system needs to resolve them in order to be able to render
    - You can shave off a little bit of time by supplying these attributes yourself, when rendering attributes strings.

- Specify alignment and writing direction if known

    ```swift
    // Only do this if you're absolutely sure your text doen't have mixed directions
    var myParagraphStype = NSMutableParagraphStyle()
    myParagraphStype.baseWritingDirection = .leftToRight
    myParagraphStype.alignment = .left
    ```

    - You might see some small improvements from explicitly specifying the writing direction and alignment, instead of using the natural settings.
    - And this will save you a little time, because the text system can skip over any logic that tries to figure out the writing direction and alignment. 
    But remember that you'll only want to do this if you're absolutely sure that your input data won't contain mixed writing directions.

- Use clipping line break mode for single line labels

    ```swift
    // Only do this if you're sure your text doesn't require wrapping
    var myParagraphStype = NSMutableParagraphStyle()
    myParagraphStype.lineBreakingMode = .byClipping
    ```

    - By default, labels will use word wrap.
    - When you do this, the text system needs to figure out where to place the line breaks.
    - You can skip this line breaking and hyphenation logic, and your text might run there just a little bit faster

    ## Summary

    ![/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2029.png](/WWDC2017/images/Efficient-Interactions-with-Frameworks/Untitled%2029.png)

    - Use the concepts of scale and frequency to minimize the large expense of operation in your code
    - Don't sweat the small infrequent stuff
    - Always measure if you aren't sure
