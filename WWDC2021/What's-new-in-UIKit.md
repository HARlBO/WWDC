# What's new in UIKit

> 🗂WWDC21 | Category : UI Framework
>
> 🔗 [https://developer.apple.com/videos/play/wwdc2021/10059/](https://developer.apple.com/videos/play/wwdc2021/10059/)
>  <br />
>  <br />
> 🔖 Discover the latest updates and improvements to UIKit and learn how to build better iPadOS, iOS, and Mac Catalyst apps. We'll take you through UI refinements, productivity updates, and API enhancements, and help you explore performance improvements and security & privacy features.

> <br />
<br />  

## Productivity

### iPad multitasking

![/WWDC2021/images/What's-new-in-UIKit/Untitled.png](/WWDC2021/images/What's-new-in-UIKit/Untitled.png)

![/WWDC2021/images/What's-new-in-UIKit/Untitled%201.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%201.png)

This will open the message in its own UIWindowScene in the center of the screen. 

![/WWDC2021/images/What's-new-in-UIKit/Untitled%202.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%202.png)

 This action takes a closure that returns an activation configuration, created with an NSUserActivity that can be handled by your app. Add this action to a context menu and you're all set.

<br /> 

### Pointer band selection

![/WWDC2021/images/What's-new-in-UIKit/Untitled%203.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%203.png)

Improved pointer support by adding band selection.

In addition to providing new API, we've enabled band selection by default for UICollectionViews that support multi-selection. 

<br /> 

### Pointer accessories

![/WWDC2021/images/What's-new-in-UIKit/Untitled%204.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%204.png)

Added pointer accessories that allow you to communicate additional context or hint at functionality by combining secondary shapes with any pointer style.

Multiple accessories can be displayed at a time and positioned around the pointer. They have the same fluid nature as the pointer, and the system seamlessly animates between different accessory shapes and positions.

<br /> 

### Keyboard shortcuts

![/WWDC2021/images/What's-new-in-UIKit/Untitled%205.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%205.png)

In iPadOS 15, completely redesigned the keyboard shorcut menu with categorized shorcuts and built-in search, finding the shortcut you're looking for has never been easier. The new Keyboard Shortcut menu also provides increased parity between iPad and  Mac Catalyst versions of  your app.

![/WWDC2021/images/What's-new-in-UIKit/Untitled%206.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%206.png)

To take full advantage of these new features, you need to adopt UIMenuBuilder. Implement buildMenuWithBuilder on your UIApplicationDelegat. Assign commands to one of the pre-defined categories such as "View" or "File" or even create your own custom category.

To use categories, you will need to audit your application for uses of UIResponder's keyCommands property.

<br /> 

### Keyboard navigation

![/WWDC2021/images/What's-new-in-UIKit/Untitled%207.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%207.png)

With UIFocusSystem on iPad and Mac Catalyst, the arrow keys are used to move between focus items and the tab key to move between focus groups.

<br /> 

### Drag and drop

With one simple gesture, you can seamlessly move data within the application and on iPadOS even between applications. With iOS 15, UIKit has enabled inter-app drag and drop on iPhone as well, unlocking many exciting new interactions.

<br /> 

## UI refinements

![/WWDC2021/images/What's-new-in-UIKit/Untitled%208.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%208.png)

![/WWDC2021/images/What's-new-in-UIKit/Untitled%209.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%209.png)

Refined the apprearance of UIToolbar and UITabBar. This updated look removes the background material when scrolled to bottom, giving more visual clarity to your content.

In UITabBar, we've enhanced support for SF Symbols, giving great results when using any of your favorite symbols.

<br /> 

### UIBarApperance

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2010.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2010.png)

You should audit your code for places where you may be setting a bar's translucent property to false and check for any UIViewControllers that have non-standard edgesForExtendedLayout.

Both of these conditions will cause visual issue with the new apperarnce. 

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2011.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2011.png)

If the new default behavior is not appropriate for your app, just create a custom appearance and assign it to the scrollEdgeAppearance property on your bar.

This property was previously only available on UINavigationBar but is now also available on UIToolbar and UITabBar.

Setting a custom appearance will avoid any of the visual issues caused by the previously mentioned imcompatible APIs.

<br /> 

### List headers

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2012.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2012.png)

`.plain`

- Section headers now display seamlessly in line iwth the content and only display a visible background material when becoming pinned to the top as you scroll down.
- In addition, there's new padding inserted above each section header to visually separate the sections with this new deisgn.
- You should use this plain style in conjunction with index bars for fast scrubbing when list content is long.

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2013.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2013.png)

`.grouped`

- UI that doesn't contain a lot of custom or visually rich conent.
- A great choice for configuration UI or registration flows similar to what you'd find in the Settings app.

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2014.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2014.png)

🆕 `.prominentInsetGrouped`

- Similar to the existing sidebar header style used for sidebar lists on iPad.
- A great choice to use when adapting a .sidebar list to an .insetGrouped list in a compact size class.
- The alarm tab in the Clock app makes great use.

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2015.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2015.png)

🆕 `.extraProminentInsetGrouped`

- For use with content that is visually rich so that headers maintain hierarchy and avoid becoming lost.
- Check out the Watch app's Face Gallery

 

To access all of these great header styles, use the UIListContentConfiguration API introduced in iOS  14. 

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2016.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2016.png)

You can specify a configuration for the entire list, or you can override the system-generated appearance on a per-row basis, giving you full control over separators.

<br /> 

### Sheet presentations

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2017.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2017.png)

Sheets in iOS 15 gain the ability to only cover half the screen, displayed at what we call the medium height detent.

You can optionally disable dimming behind this detent to create non-modal experience allowing interaction both within and behind the sheet.

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2018.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2018.png)

Now, you can simply tap the time to use the keyboard for input.

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2019.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2019.png)

And with Magic Keyboard on iPad, you can even edit the time right in-line.

<br /> 

## API enhancements

### Enhanced UIButton API

flexibly configure button's look and feel

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2020.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2020.png)

support resizing in response to the system text size setting known as Dynamic Type, and for the first time formally support multi-line text

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2021.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2021.png)

UIButtonConfiguration allows you to make pop-up and pull-down buttons natively in UIKit for the fist time.

 

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2022.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2022.png)

<br /> 

### Submenus

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2023.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2023.png)

<br /> 

### SF Symbol enhancements

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2024.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2024.png)

the ability to use colors in three new ways:

→ Hierarchical, Palette, Multicolor

New APIs for using all these colorful modes are available in UIKit, SwiftUI and AppKit.

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2025.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2025.png)

<br /> 

### SF Symbol variants

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2026.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2026.png)

filled, on circles or on rectangles

In previous release, these are selected by specifying dotted strings.

<br /> 

### Content size category limits

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2027.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2027.png)

UIContentSizeCategory traits represent the system text size setting, also called the dynamic type size, in code.

You can set labels, textfields, textviews, and image views perhaps containing SFSymbols to automatically adjust to the setting.

In iOS 15, we've added a new way to restrict how the traits are applied to view hierarchies.

This enables you to easily set a floor or ceiling for the size.

<br /> 

### UIColor enhancements

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2028.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2028.png)

We've unified the system colors across all of our platforms.

<br /> 

### Dynamic tint color

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2029.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2029.png)

It's a color that's resolved at runtime, based on the app or trait hierarch's current tint color.

It's perfect for using with the new UIButton.Configuration, and the nuew colorful SF Symbols APIs.

<br />  

### Color picker enhancements

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2030.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2030.png)

callback that allows app UI to be updated as the color is mixed and changed, as well as when the picking is complete.

<br /> 

### TextKit2

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2031.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2031.png)

TextKit 2 is the new, next-generation text layout system.

It's a powerful new system that makes it easier to express what you want to do with text, and it does it in a fast high performance way.

UIKit has adopted it behind the scenes to power UITexxtField, where brings better layout to text in languages with complex scripts, like Kannada with no adoption required.

<br /> 

### UIScene state restoration

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2032.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2032.png)

A UISceneSession represents an insatnce of  yhour application's UI, and corresponds to an app window represented in the app switcher.

Interface state is represented by an NSUserActivity. Your ap provides this NSUserActivity to the system when a scene enters the background, and should use it to restore the interface state when the scene is reinstantiated. 

There's a new way to get and set the transient state of our text input views.

There's a new UIScene callback that provides a more convenient place to restore state after a storyboard loads. And there's an opportunity to extend the app launch process and delay when app's UI becomes active if you have asynchronous model code that returns state. 

If you're still using the old UIApplication-based lifecycle from before UIScene was introduced in iPadOS 13, now is the time to switch to UIScene.

All UIKit apps can use it. Supporting multiple windows is not requered, although, for iPad and Mac apps.

<br /> 

### Scene level sharingnew A

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2033.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2033.png)

New APIs that allow apps to represent the currently sharable content that's being interacted with in each scene.

<br /> 

### Cell configuration closures

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2034.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2034.png)

In iOS 15, new closure-based update handlers make it easier than ever to reconfigure your cells.

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2035.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2035.png)

No longer you need to create a cell subclass and override updateConfiguraton using state. You can now write that code inline, in the same place you create the cells.

Similar closure-based functions are available in the new UIButtonConfiguration APIs too.

<br /> 

### Diffable data source improvements

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2036.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2036.png)

We've improved Diffable data source to make it easier to update your collection and table views.

In iOS 15, when you apply a snapshot without animating differences, the UI updates based on those changes without discarding all the existing cells.

And there's new API to effiently reconfigure items, so you can update the content displayed in existing cells when the properties of items change without their identity changing. 

<br /> 

## Performance

Every device that UIKit runs on has multiple processor cores, and powerful graphics hardware. Thins should happen fast. Animations and scrolling should always be smooth.

In iOS 15, there are a few enhancements and new APIs that make buildings apps with these characteristics even easier.

<br /> 

### Cell prefetching improvements

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2037.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2037.png)

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2038.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2038.png)

Many cells show images. In the past, you might have noticed momentary interruptions in scrolling when the main UI queue is tied up decoding large images.

In iOS 15, app code can take more control over this process. There are new easy functions to prepare images so they're completely ready when your app needs to display them. 

And these functions are easy to use asynchronously, so the UI queue can be free to process events while the images are being decoded. 

Many apps handle large images, but display them at small sizes. To help with this, there are new UIImage APIs that resize images more efficiently and save memory by using the system's knowledge about the images and the display.

<br /> 

### Swift async/await

Most UIKit APIs must be called on the main UI queue, and we've annotated those APIs as Main Actor to ensure that this is enforced, for the first time, at compile time.

In other areas, like the new UIImage preparation features, we've tweaked or APIs to ensure that UIKit is easy and safe to use with the new asynchronous Swift language features. 

<br /> 

## Security and privacy

Allows the system to verify what interface is really being interacted with. We've intergrated this into UIKit in a few places.

<br /> 

### Location Button

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2039.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2039.png)

New APIs allow apps to embed buttons that grant case-by-case, one-time access to the device's current location. They do this when, and only when, they are tapped on without lots of alerts or prompts.

The API is flexible, so it can match every app's look, but behind the scenes, it ensures buttons are always clear and legible, or they won't work.

 

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2040.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2040.png)

In iOS 15, we're elimination the banner any time the system can confirm that the data was accessed after deliberate interaction. with a standard system paste interface.

For example, a tap on the paste button in the editing menu, or a cmd-V on a hardware keyboard.

<br /> 

### Standard paste items

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2041.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2041.png)

Added API to provide few new standard Paaste menu items. When there are used, the notification banner is also not displayed.

For each of these, there are standard UIReponder selectors for use with UIMenuController and UICommand and new identifiers for use with UIAction.

Sometimes an app wants more information about what's on the pasteboard, but doesn't need full access. In iOS 14, we introduced an API that apps can use to check if there is a number, probable web URL, or probable web search term on the pasteboard. And we use these in Caluator and Safari. 

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2042.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2042.png)

In iOS 15, this API has been greatly expanded to cover all the standard Data Detectors type. None of these will show the notice, because they don't grant access to the data itself.

There are also APIs to retrieve the data values without having to parse the text yourself, although if these APIs used at any time other than after the use of a standard paste interface, the system will show the paste notice.

<br /> 

### Private Click Measurement

![/WWDC2021/images/What's-new-in-UIKit/Untitled%2043.png](/WWDC2021/images/What's-new-in-UIKit/Untitled%2043.png)

UIEventAttribution was developed in conjuction with the WebKit team. WebKit's Private Click Measurement feature provides Web-to-Web Click Measurement. UIEventAttribution brings PCM to UIKit, and provides App-to-Web Click Measurement. This means privacy-preserving measurement of ad clicks and taps.

<br />

## Wrap up

- Compile your app for iOS 15 SDK
- Check out new system features
- Adopt the new iOS 15 look
- Use the new APIs
