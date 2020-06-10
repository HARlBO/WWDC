# Delivering Intuitive Media Playback with AVKit

>  📅 2020.6.4 (THU)
>
> 🗂 WWDC2019 | Session : 503 | Category : AVKit
> 
> 🔗 [https://developer.apple.com/videos/play/wwdc2019/503/](https://developer.apple.com/videos/play/wwdc2019/503/)
> <br/>
> <br/>
>🔖 AVKit is a high-level framework for building media user interfaces, complete with playback controls, chapter navigation, Picture-in-Picture, audio routing, support for subtitles and closed captioning, Siri and Now Playing integration, and support for keyboard, Touch Bar, and remote control. Learn best practices in how to integrate these technologies into your own apps on iOS, tvOS, and iPad apps on Mac.

## AVKit in the Stack

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled.png)

Media Playback with AVPlayerViewController on iOS and tvOS

```swift
import AVKit

// 1) Create an AVPlayer
let player = AVPlayer(url: "http://my.example/video.m3u8")

// 2) Create an AVPlayerViewController
let playerViewController = AVPlayerViewController()
playerViewController.player = player

// 3) Show it
present(playerViewController, animated: true)
```

# Media playback with AVKit

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%201.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%201.png)

When you use AVKit, you own the media playback objects. You are in control of creating and managing them.

And video rendering is build in on top of same core technology that powers AVPlayerLayer.

## AVPlayerViewController on iOS

### 🆕 Full screen callbacks

- Extends `AVPlayerViewControllerDelegate`
- Notifies delegate when beginning or ending full screen presentation

    ```swift
    @available(iOS 12.0, *)

    func playerViewController(_ playerViewController: AVPlayerViewController, 
    				willBeginFullScreenPresentationWithAnimationCoordinator coordinator: 
    				UIViewControllerTransitionCoordinator)

    func playerViewController(_ playerViewController: AVPlayerViewController, 
    				willEndFullScreenPresentationWithAnimationCoordinator coordinator: 
    				UIViewControllerTransitionCoordinator)
    ```

Implementing AVPlayerControllerDelegate's full screen callbacks

```swift
fund playerViewController(_ playerViewController: AVPlayerViewController, 
				willBeginFullScreenPresentationWithAnimationCoordinator coordinator: 
				UIViewControllerTransitionCoordinator) {
	coordinator.animate(alongsideTransition: { (context) in
		// Add coordinated animations
	)} { (context) in
		if context.isCancelled {
			// Still embedded inline
		} else {
			// Presented full screen
			// Take strong reference to playerViewControllelr if needed
		}
}
```

`alongsideTransition` calback 

→ This is where you find out  whether a transition succeeded or whether the user cancelled. This is the source of truth for learning about entering full screen from an inline presentation.

#### 🆕 Full Screen Callbacks

##### Retargeting dismissal animaion

- Keep embedded playerViewController alive when full screen
    - Deallocating will stop full screen playback
    - Okay to be offscreen or removed from superview / parent
- Use delegate to restore playerViewController or retarget animation

    ```swift
    func playerViewController(_ playerViewController: AVPlayerViewController, 
    				willEndFullScreenPresentationWithAnimationCoordinator coordinator: 
    				UIViewControllerTransitionCoordinator) {
    	// If scrolled away, update playerViewController's layout here
    }
    ```

- Track full screen presentation state with `AVPlayerViewControllerDelegate`
    - Do not rely on `viewWillAppear(_:)` and friends
- Works for both embedded and full screen presentations

### 🆕 AVPlayerViewController in iPad Apps on the Mac

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%202.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%202.png)

You get the same UI that you expect from Apple's playback UIs with all the functionality that AVPlayerController delivers for iPad apps.

But you also get some platform specific features for Mac. (touch bar support, keyboard support, picture-in-picture support)

→ How many new lines of code? 0!

- Exact same API an os iOS
- Picture-in-picture support
    - Includes AVPictureInPictureController (also for AppKit)
    - And AVPlayerView, for AppKit-based apps
- Touch Bar, keyboard, and Now Playing support
- Audio and AirPlay routing
- Available in macOS 10.15

### 🆕 External Metadata

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%203.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%203.png)

### 🆕 Improved Support For Custom Controls

- Interactive dismissals
- Landscape support for portrait-only apps
- Keyboard and Touch Bar support
- Now Playing management
- Automatic video zoom

### 🆕 Custom Playback Controls

- Set `showsPlaybackControls` to `false`
- Present modally
- Add controls to `contentOverlayView`
- For the best user experience, you should:
    - Override UIViewController methods for status bar, home inidicator
    - Pass unhandled touches through your view
    - Let AVKit handle double-tap for video zoom

## Best practices

#### Showing Full Screen

- Covers `UIWindowScene` coordinate space
- Use cases:
    - Splash screen
    - Full screen playback

##### Splash Screen

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%204.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%204.png)

**UIViewController containment API**

- Add as child:

    ```swift
    parent.addChild(playerViewController)
    parent.view.addSubView(playerViewControler.view) // Or other UIView insertion API
    //Enable auto layout and set up constraints
    playerViewController.didMove(toParent: parent)
    ```

- Remove from parent:

    ```swift
    playerViewController.willMove(toParent: nil)
    playerViewControoler.view.removeFromSuperview()
    playerViewController.removeFromParent()
    ```

**Configure AVPlayerViewController**

- Disable playback controls:

    ```swift
    playerViewController.showsPlyabackControls = false
    ```

- Make video fill entire screen:

    ```swift
    playerViewController.videoGravity = .resizeAspectFill
    ```

- Set background color (if needed):

    ```swift
    playerViewController.view.backgroundColor = .clear
    ```

**AVFoundation best practices**

- Disable `AVPlayer.allowsExternalPlayback`
- Configure `AVAudioSession` for secondary media playback
    - Use `.ambient` category
- Observe `AVAudioSession.silenceSecondaryAudioHintNotification`
    - Honor `AVAudioSession.secondaryAudioShouldBeSilencedHint`

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%205.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%205.png)

##### Full Screen Playback

**AVPlayerViewController best practices**

- Present modally 
(not as childViewController of other ViewController)
- Use the default `modalPresentationStyle`
- Do not set `AVPlayerViewController.videoGravity`
- Use `AVPlayerViewController` to track full screen presentation state
    - Do not rely on `viewWillAppear(:_)` and friends

- Set up `AVPlayerItem` before buffering
    - Set `AVPlayer.rate` for setting item
- Observe `AVPlayer.status` and `AVPlayerItem.status`
    - Don't begin playback until status is `.readyToPlay`
    - Check `error` property when status is `.failed`
        - Rebuild AVFoundation objects if `.mediaServicesWereReset`
- Enable `AVPlayer.usesExternalPlaybackWhileExternalScreenIsActive`
- Configure `AVAudioSession` for `.playback`

### Embedding AVPlayerViewController InLine

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%206.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%206.png)

#### Showing poster images

- Use `.contentOverlayView`
    - Okay to use before setting player or player item
- Observe `.isReadyForDisplay`
    - Returns `true` when first frame is ready

```swift
let token = pvc.observe(\.isReadyForDisplay, options: [.initil]) {
								[weak self] observed, _ in
	if observed.isReadyForDisplay {
		// Hide any poster frame or placeholder
	}
}

```

### Picture-in-picture

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%207.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%207.png)

Continue playing when entering background. If you must pause, wail until in background

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%208.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%208.png)

Restoring from picture-in-picture using AVPlayerViewControllerDelegate

```swift
func playerViewController(
		_ playerViewController: AVPlayerViewController,
		restoreUserInterfaceForPictureInPictureStopWithCompletionHandler
		completionHandler: @escaping (Bool) -> Void) {
	presentingViewController.present(playerViewController, animated: true) {
		// Must invoke completionHandler
		completionHandler(true)
	}
}
```

---

## What AVPlayerViewController Provides on tvOS

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%209.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%209.png)

### 🆕Custom interactive overlays

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%2010.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%2010.png)

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%2011.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%2011.png)

Set the `customOverlayViewController` property of the playerViewController

```swift
let customInteractiveVideoOverlay = UIViewController(nibName: "CustomInteractiveVideoOverlay", bundle: nil)
newPlayerViewController.customOverayViewController = customInteractiveVideoOverlay
```

### 🆕 Live broadcast channel-flipping

- For live streams
- Swipe horizontally for next / previous channels
- Channel interstitial screen describes a channel with it loads

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%2012.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%2012.png)

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%2013.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%2013.png)

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%2014.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%2014.png)

### 🆕 Parental content restriction enforcement

![/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%2015.png](/WWDC2019/images/Delivering-Intuitive-Media-Playback-with-AVKit/Untitled%2015.png)

### Best Practices on tvOS

- Controls revealed by swipe-up? Use a custom overlay instead
- Use the delegate dismissal notifications rather than a `UITapGestureRecognizer`
- Avoid toggling `showPlaybackControls` during presentation
- Provide media content ratings
- Test with parental content restrictions enabled
