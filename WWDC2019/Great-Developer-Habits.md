# Great Developer Habits

> 📅 2020.6.19 (FRI)
> 
> 🗂WWDC2019 | Session : 239 | Category : App Frameworks
>
> 🔗 [https://developer.apple.com/videos/play/wwdc2019/239/](https://developer.apple.com/videos/play/wwdc2019/239/)
>  <br />
>  <br />
> 🔖 Successful app development requires mastering a lot of different things. Discover practices you can incorporate into your development workflow to enhance your productivity, and improve your app's performance and stability. Learn how to improve the quality of code you write with Xcode. Gain a practical understanding of some valuable development techniques.
> <br />
<br />   

### craft

> \'kraft\

1. skill in planning, making, or executing
2. to make or produce with care, skill, or ingenuity

### Hidden details matter.

## Organize

![/WWDC2019/images/Great-Developer-Habits/Untitled.png](/WWDC2019/images/Great-Developer-Habits/Untitled.png)

Groups are best used to organize your project functionally in a way that logically follows how someone might interact with your application

It can really help to make sure that your Xcode project structure and your file system structure match each other.

### Storyboards

![/WWDC2019/images/Great-Developer-Habits/Untitled%201.png](/WWDC2019/images/Great-Developer-Habits/Untitled%201.png)

Use a different storyboard file for each major section of your application and then use references to tie them together.

### Modern

Keeping your project file modern is a critical way to make sure that Xcode can help you out and avoids the accumulation of issues.

![/WWDC2019/images/Great-Developer-Habits/Untitled%202.png](/WWDC2019/images/Great-Developer-Habits/Untitled%202.png)

1. We recommend you to update your project file to the latest format which Xcode will offer when you're updating to a new version of Xcode.

![/WWDC2019/images/Great-Developer-Habits/Untitled%203.png](/WWDC2019/images/Great-Developer-Habits/Untitled%203.png)

2. Make sure that your project is using the new Xcode build system.
It offers significant improvements in performance, dependency management, and it's absolutely critical to your adoption of Swift packages.

Get rid of that unneeded and unused code.

Code should never be checked in that contains warnings. Just fix them as you go along.

**Organize**

- Functional organization with groups
- Mirror Project structure and file structure
- Break apart large storyboards
- Modernize your project file
- Address the root cause of warnings

## Track

![/WWDC2019/images/Great-Developer-Habits/Untitled%204.png](/WWDC2019/images/Great-Developer-Habits/Untitled%204.png)

1. Keep your commit small
Check your code into your working branch regularly in small increments.
2. Write useful commit messages 

- User source control
- Keep commits small and isolated
- Write useful commit messages
- Utilize branches for bug and feature work

## Document

![/WWDC2019/images/Great-Developer-Habits/Untitled%205.png](/WWDC2019/images/Great-Developer-Habits/Untitled%205.png)

Just place your cursor on the first line of the function signature, press `option + cmd + slash` and all the placeholder text you need will be generated automatically.

- Comments are critical for future understanding
- Good comments provide background and reasoning
- Use descriptive variable and constant names
- Include documentation

## Test

### Unit test

- Write unit tests
- Run unit tests before committing code
- Build a foundation for continuous integration

## Analyze

![/WWDC2019/images/Great-Developer-Habits/Untitled%206.png](/WWDC2019/images/Great-Developer-Habits/Untitled%206.png)

**Network Link Conditioner**, you can artificially constrain your network performance to one similar to that of a typical cellular network.

![/WWDC2019/images/Great-Developer-Habits/Untitled%207.png](/WWDC2019/images/Great-Developer-Habits/Untitled%207.png)

Inside of your scheme settings, there's also several sanitizers and checkers that can help discover various issues throughout your development cycle.

- **Address Sanitizer**
    - Watch for things like m emory corruptions and buffer overflows
    - Memory issues are frequently the cause of security vulnerabilities. So using Address Sanitizer, will help you make sure that you don't ship these.
- **Thread Sanitizer**
    - (while testing and debugging your app in the simulator) you can help discover data races.
    - Data races have two threads that are not synchronized and at least one of those two threads is attempting to do a write on the same piece of data.
- **Undefined Behavior Sanitizer**
    - Captures bug like dividing by zero, out of range casts between floating point types, overflows and misaligned pointers.
    - It might act in unpredictable ways, or it might act like it has no problem at all with different results at different times seemingly with no reason.
- **Main Thread Checker**
    - Ensures that you're not performing invalid usage of appKit, UIKit, and other API's on background threads.

![/WWDC2019/images/Great-Developer-Habits/Untitled%208.png](/WWDC2019/images/Great-Developer-Habits/Untitled%208.png)

While debugging your apps, keep an eye on performance and resource utilization. 

And make sure your app is being as efficient with system resources as possible.

1. Use the Debug Gauges.
(in the debug navigator in Xcode anytime you've built and run project)
You can check out CPU, memory, disk, and network utilization throughout the lifecycle of your app, quickly understand if your app is doing something like connection to unexpected servers over the network.

![/WWDC2019/images/Great-Developer-Habits/Untitled%209.png](/WWDC2019/images/Great-Developer-Habits/Untitled%209.png)

2. Take this even further, by clicking in the profile and instruments button which will allow you to run an even more in-depth analysis.

**Analyze**

- Simulate poor networks with Network Link Conditioner
- Use sanitizers and checkers
- Measure performance and efficiency with Debug Gauges
- Investigate issues with Instruments

## Evaluate

- Include code review as part of your practice
- Understand each line
- Build it
- Run tests
- Proofread for style, spelling, and syntax

Even though it might feel like this process is slowing you down in the short-term, it will undoubtedly save you time, money, and customers in the future through the reduction of potential errors and issues in the long-run.

## Decouple

![/WWDC2019/images/Great-Developer-Habits/Untitled%2010.png](/WWDC2019/images/Great-Developer-Habits/Untitled%2010.png)

Packages and frameworks offer an opportunity to maintain that code in more centralized way.\

If your app includes extensions, your binary size will actually reduce because both you main app and your extensions can actually share that same framework.

![/WWDC2019/images/Great-Developer-Habits/Untitled%2011.png](/WWDC2019/images/Great-Developer-Habits/Untitled%2011.png)

The code that lives in your app, shared frameworks, packages, and libraries need to be accompanied by great documentation, in order to useful to others.

- Determine functional segments and break them out
- Scale your work across multiple apps
- Improve efficiency with extensions
- Share you efforts with the broader community
- Documentation is critical

## Manage

- Use community and open source projects responsibly
- Understand dependencies thoroughly
- Ensure that privacy is respected
- Have a plan if a dependency goes away or is no longer maintained
