# Architecting Your App for Multiple Windows

> 🗂 WWDC2019 | Session : 258 | Category : iPad
> 
> 🔗 [https://developer.apple.com/videos/play/wwdc2019/258](https://developer.apple.com/videos/play/wwdc2019/258/)
>  <br />
>  <br />
> 🔖 Dive into the details about what it means to support multitasking in iOS 13. Understand how previous best practices fit together with new ideas. Learn the nuances of structuring your application to support multiple windows, and how to instantiate your UI, handle windows coming and going, and manage your app's underlying window resources.
> <br />
<br />  


## Changes to app lifecycle

### App Delegate Responsibilities

**iOS 12 and earlier**

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled.png)

AppDelegate 역할 (iOS 12 and eariler)

1. notify application of process level events
2. letting application know the state of its UI

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%201.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%201.png)

Application has one process and also just one user interface instance to match it.

**iOS 13**

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%202.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%202.png)

Application now still just share one process but may have multiple user interface instances or scene sessions.

### 🆕 App Delegate Changes

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%203.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%203.png)

It's still reponsible for process events and life cycle,

but it's no longer reponsible for anything related to your UI lifecycle.

Instead, that'll all be handled by UISceneDelegate.

→ Any UI setup or teardown work needs to migrate to to corresponding methods in your Scene Delegate.

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%204.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%204.png)

If your application adopts the new scene lifecycle, UIKit will stop calling the old AppDelegate methods that relate to UI state.

If you want to adopt multiple windows support on iOS 13, that doesn't mean you need to drop support for 12 and before.

If you're back deploying, you can simply keep both sets of these methods and UIKit will call the correct set at runtime.

### 🆕 Session Lifecycle

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%205.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%205.png)

One more additional reponsibility

→ the system will now notify your AppDelegate when a new Scene Session is being created, or an existing Scnene Session is being discarded

앱을 터치하면

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%206.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%206.png)

Before it creates the actual UI Scene, it's going to ask our application for UIScene configuration.

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%207.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%207.png)

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%208.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%208.png)

This configuartion specifies what scene delegate, what story board, and if you specifed, what scene subclass you want to create the scene with.

Now our app is launched. We have a scene session.

But we don't see any UI, and that's where our scene delgates did connect to scene session comes in.

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%209.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%209.png)

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2010.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2010.png)

 

Now we see our app.

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2011.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2011.png)

User swipes up to go back home

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2012.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2012.png)

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2013.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2013.png)

 At some point in time after, your scene may be disconnected.

In order to reclaim resources, the system may at some point after your scene enters the background, release that scene from memory.

That also means your scene delegate will be released from memory and any window hierarchies or view hierarchies held by your scene delegate will be released as well.

But it's important to not use this to actually delete any user data or state permanently, as the scene may reconnect and return later.

 

User swipes up on a scene session on switch and explicitly wants to destory it

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2014.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2014.png)

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2015.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2015.png)

This finally gives us an opportunity to actually permanently delegate any user state or data associated with the scene such as an unsaved draft in a text editing app.

## State Restoration

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2016.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2016.png)

At some point, these other two background road trip and attendee scenes, have been disconnected and release by the system.

If you don't implement state restoration here, when you go back to the read trip, you're not going to return to the state that previously in.

Instead, start over like it's a brand-new window, and that's now a great user experience.

### 🆕 Per-Scene State Retoration

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2017.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2017.png)

It works by not any more encoding view hierarchies, but instead just encoding the state which will allow you to recreate your window.

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2018.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2018.png)

## Keeping Scene in Sync

How to best keep application scene in sync.

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2019.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2019.png)

Only one of scenes updated.

So why is that?

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2020.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2020.png)

1. View controllers recieve an event, may be via button tap, pressing the send button.
2. View controllers itself updates its own UI.
3. View controllers notifies our model or model controller.

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2021.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2021.png)

1. Architecturally, now if our view controllers, upon receiving an event, immediately and only notify our model controller, 
2. then we can have our model controller actually notify any relevant subscribers or view controllers, telling them that they should update with this new data

**→ delegate, notification, Combine**

![/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2022.png](/WWDC2019/images/Architecting-Your-App-for-Multiple-Windows/Untitled%2022.png)
