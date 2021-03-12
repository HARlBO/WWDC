# Window Management in Your Multitasking App

> 🗂WWDC19 | Session : 246 | Category : 
> 
> 🔗[https://developer.apple.com/videos/play/wwdc2019/246/](https://developer.apple.com/videos/play/wwdc2019/246/)
>  <br />
>  <br />
> 🔖 Dive into the details of window management in your Multitasking app, including how to properly handle creating, refreshing, and closing windows. Hear about best practices for when to refresh the content in your window and learn how to ensure your app's visual state is up-to-date in the switcher.
> <br />
<br />  

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled.png)

## Activate

**Activate a seesion only in reponse to direct and local user interaction**

### Session Activation

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%201.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%201.png)

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%202.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%202.png)

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%203.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%203.png)

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%204.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%204.png)

1. UIKit creates a brand-new scene session.
2. And let's you specify a configuration for it, by calling `configurationForConnectingSceneSession` 
- You can inspect the user activity which is given back to you now through the UI scene connecting options, to pick a session

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%205.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%205.png)

`sceneWillConnectToSession:`

you are able to find your user activity and the connecting options, configure a window and view controller hierarchy for it.

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%206.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%206.png)

`sceneWillConnectToSession:` 

If the session is still existing, we go straight to the scene delegate.

If the session had been disconnected in the meantime.

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%207.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%207.png)

`continueUserActivity:`

If the scene is still connected

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%208.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%208.png)

## Refresh

**Refresh a session for user-relevant updates**

### Session Refresh

- Updates to shared data
- New user-relevant data
- Session and scene hygiene

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%209.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%209.png)

- State restoration user activity
- Scene Activation conditions
- Session UI and switcher snapshot

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2010.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2010.png)

If the scene is still connected, either in the foreground or the background, it can listen to the notification itself and call the API, which is going to do the right thing internally.

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2011.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2011.png)

If the scene has been disconnected, the view controller won't be there anymore. And so, the long-libed object can step in, figure that out and call to refresh API in its place. The scene is goint then to be background connected and the view controller will have a chance to update itself and snapshot will be captured.

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2012.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2012.png)

## Destroy

**Destroy a session only in response to direct user interaction**

### Session Destruction

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2013.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2013.png)

**Use the animation to acknowledge the user's intent**

### Session Destruction Animations

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2014.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2014.png)

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2015.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2015.png)

## Summary

![/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2016.png](/WWDC2019/images/Window-Management-in-Your-Multitasking-App/Untitled%2016.png)
