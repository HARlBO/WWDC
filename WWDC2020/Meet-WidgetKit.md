# Meet WidgetKit

> 📅 2020.6.29 (MON)
>
> 🗂 WWDC2020 | Session : 10028 | Category : Frameworks
>
> 🔗 [https://developer.apple.com/videos/play/wwdc2020/10028/](https://developer.apple.com/videos/play/wwdc2020/10028/)
>  <br />
>  <br />
> 🔖 Meet WidgetKit: the best way to bring your app's most useful information directly to the home screen. We'll show you what makes a great widget and take a look at WidgetKit's features and functionality. Learn how to get started creating a widget, and find out how WidgetKit leverages the power of SwiftUI to provide a stateless experience. Discover how to harness your existing proactive technologies to make sure your widget surfaces relevant material. And create a Timeline that ensures your content is always fresh. For more on creating widgets, check out "Build SwiftUI views for widgets" and "The widgets code-along."
 and explore the growing number of APIs available as Swift Packages.
 > <br />
 <br />  
 

In iOS 14, we have a dramatic new Home screen experience, one that is much more dynamic and personalized, with a focus on widgets. 

New widgets are designed to be bold, highly glanceable, and be perfectly at home not just on the iPhone Home screen but also our refreshed Today view, pinned to your iPad home screen, and finally in the gorgeous new Notification Center on macOS Big Sur.

## What makes a great widget?

![/WWDC2020/images/Meet-WidgetKit/Untitled.png](/WWDC2020/images/Meet-WidgetKit/Untitled.png)

### Glanceable

![/WWDC2020/images/Meet-WidgetKit/Untitled%201.png](/WWDC2020/images/Meet-WidgetKit/Untitled%201.png)

New widgets can come in many sizes. Especially at the smallest widget size, you only have the space of around four Home screen icons. You don't need to tap any buttons or even spend time trying to figure out a complicated UI. The content is the focus. 

- Widgets are not mini-apps.

> Design Great Widgets - WWDC20

### Relevant

**Smart Stacks**

Smart stacks are a collection of widgets that will automatically rotate to show the right widget at the top. But you can also swipe through.

- Stacks use on-device intelligence
- Siri Shortcuts donation
- WidgetKit API

> Add Configuration and Intelligence to Your Widgets - WWDC20

### Personalization

![/WWDC2020/images/Meet-WidgetKit/Untitled%202.png](/WWDC2020/images/Meet-WidgetKit/Untitled%202.png)

Different metric

3 different sizes : small, medium, or large  

→ You're not required to support all the sizes, but recommended supporting as many sizes as possible

**WidgetKit** 

We can generate this entire configuration UI from your Intent completely automatically, with no additional work for you.

## WidgetKit

![/WWDC2020/images/Meet-WidgetKit/Untitled%203.png](/WWDC2020/images/Meet-WidgetKit/Untitled%203.png)

It was a goal from the very beginning for widgets to be multi-platform and to make it as easy as possible for developers to have their learnings apply across iOS, iPadOS, and macOS.

So widgets user interface in WidgetKit are built entirely with SwiftUI.

SwiftUI also makes it easy to support features like Dynamic Type and Dark Mode nearly automatically.

![/WWDC2020/images/Meet-WidgetKit/Untitled%204.png](/WWDC2020/images/Meet-WidgetKit/Untitled%204.png)

The average person goes to the Home Screen more than 90 times a day and only spends a few moments there.

When we designed complications on watchOS, we had very similar goals of having things be ready and immediately glanceable. So we took some inspiration from how they were built.

### How WidgetKit works

![/WWDC2020/images/Meet-WidgetKit/Untitled%205.png](/WWDC2020/images/Meet-WidgetKit/Untitled%205.png)

WidgetKit extension are background extensions that returns a series of view hierarchies in a timeline.

Using the declarative nature of SwiftUI, we can package up these views in this timeline and send them over to the Home screen which will present them at the correct time according to the timeline. 

This avoids the entire "launch a process, load, and then present a view". They are ready to go and immediately glanceable. 

![/WWDC2020/images/Meet-WidgetKit/Untitled%206.png](/WWDC2020/images/Meet-WidgetKit/Untitled%206.png)

The fact that we have your views ready beforehand means we can also reuse them in other areas of the system.

For example, we can have this super-fun experience to add widget from the Widget Gallery, where people who use your app can get a preview of exactly what your widget will look like right on the Home screen.

![/WWDC2020/images/Meet-WidgetKit/Untitled%207.png](/WWDC2020/images/Meet-WidgetKit/Untitled%207.png)

**Example Calendar's widget**

![/WWDC2020/images/Meet-WidgetKit/Untitled%208.png](/WWDC2020/images/Meet-WidgetKit/Untitled%208.png)

The extension can use this information to render the right views for when my next meeting is happening, when I'm in that meeting, and the next one after.

If I go into Calendar and update the event, Calendar will then use API to reload the timeline.

What we mean by that is that the extension wakes up and return a new timeline with all the new updates.

## Defining a Widget

![/WWDC2020/images/Meet-WidgetKit/Untitled%209.png](/WWDC2020/images/Meet-WidgetKit/Untitled%209.png)

We wanted a mechanism to allow a single extension to support multiple kinds of widgets.

![/WWDC2020/images/Meet-WidgetKit/Untitled%2010.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2010.png)

For example, a single Stocks extension provides an experience like the Stocks overview widget, a great widget which provides glanceable information about a few stocks. But also, that same extension powers the Stock detail widget which allow a user to show a single stock on their Home screen or Notification Center on mackOS.

WidgetKit extension can support SwiftUI, AppKit, and Catalyst-based macOS apps. 

### Configuration

![/WWDC2020/images/Meet-WidgetKit/Untitled%2011.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2011.png)

Kinds of widgets can also express which type of configuration they support.

#### StaticConfiguration

![/WWDC2020/images/Meet-WidgetKit/Untitled%2012.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2012.png)

#### IntentConfiguration

![/WWDC2020/images/Meet-WidgetKit/Untitled%2013.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2013.png)

### supportedFamilies

![/WWDC2020/images/Meet-WidgetKit/Untitled%2014.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2014.png)

A particular kind can also enable one or many supportedFamilies. By default, widgets support all family types.

![/WWDC2020/images/Meet-WidgetKit/Untitled%2015.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2015.png)

![/WWDC2020/images/Meet-WidgetKit/Untitled%2016.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2016.png)

These families look great on iOS and on macOS.

### Placeholder

![/WWDC2020/images/Meet-WidgetKit/Untitled%2017.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2017.png)

Great placeholder UIs show a representation of what your kind of widget is. 

- Default content
Each kind of widget is required to provide a placeholder UI. Placeholder UI is the default content of your widget. It should be a representation of your widget kind, but nothing more than that.
- No user data
There should not be any user data in this UI.
- Queried on environment change
The other important thing to note is this UI retrieved only sparingly. There are no guarantees on when that will occur. Typically we will only ask for a new placeholder UI on a device environment change.

Sample Widget code

![/WWDC2020/images/Meet-WidgetKit/Untitled%2018.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2018.png)

## Creating a glanceable experience

![/WWDC2020/images/Meet-WidgetKit/Untitled%2019.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2019.png)

All three show me useful information and invite me as the user to tap to launch the app and find out more information. The first aspect of creating a glanceable experience is creating StatelessUI, which SwiftUI is uniquely perfect for.

### StatelessUI

- Widgets are not mini-apps
- No scrolling
- No videos or animated images
- Tap interactions

### Deep links

![/WWDC2020/images/Meet-WidgetKit/Untitled%2020.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2020.png)

User can tap on the most recently played album and deep link directly into that app. systemSmall has a single tap target, so the entire widget is a tap target meant to tkae the user directly into the app.

![/WWDC2020/images/Meet-WidgetKit/Untitled%2021.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2021.png)

Each album is an individual link which can take you directly into that app.

### Tap interactions

![/WWDC2020/images/Meet-WidgetKit/Untitled%2022.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2022.png)

The entire widget can be associated with a URL link using the widgetURL API.

![/WWDC2020/images/Meet-WidgetKit/Untitled%2023.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2023.png)

If you want to create sublinks in systemMedium or systemLarge, then you can use the new Link API in Swift UI.

## Views, timelines, and reloads

### Views

- Placeholder
- Snapshot
- Timeline

#### Spatshot

![/WWDC2020/images/Meet-WidgetKit/Untitled%2024.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2024.png)

Snapshot is where the system needs to quickly display a single entry. So the expectation is for your extension to quickly return a view as fast as possible because when you do so, you'll see your real widget in the gorgeous Widget Gallery on iOS.

This isn't a screenshot or image we had to provide at design time. This is your real widget experience on iOS, iPadOS, and macOS.

#### Timeline

![/WWDC2020/images/Meet-WidgetKit/Untitled%2025.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2025.png)

What you see in the Widget Gallery is what you get when the user adds it to their device/

If a snapshot is just a single entry, then a series of multiple views to be displayed at the right time is just a timeline.

Timelines are a combination of views and dates that are returned, which allow you to say at what time a particular view should be shown.

![/WWDC2020/images/Meet-WidgetKit/Untitled%2026.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2026.png)

And by returning a timeline is how we drive the widget experience. Timeline's returned should be for both Dark and Light Mode. 

When a WidgetKit extension returns an entry, we will take that information and serialize the view hierarchy to disk. This means we just-in-time render each individual entry. This enables the system to power numerous widgets at the same time with numerous timelines.

Timelines should typically be returned for a day's worth of content. There are times though when your widget needs to return more up-to-date information. 

We do this through the concept of what we call reloads.

### Reloads

![/WWDC2020/images/Meet-WidgetKit/Untitled%2027.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2027.png)

Reloads are where the system will wake up your extension and ask for a new timeline for each widget placed on the device.

Reloads help ensure that your content is always up-to-date for your user.

`TimelineProvider` protocol

![/WWDC2020/images/Meet-WidgetKit/Untitled%2028.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2028.png)

`TimelineEntry` : consists mainly of a date

`Context` : provides environment information and the context for which the system is asking you for entries

`func snapshot` : the system asks for a single entry

`func timeline` : the system asks for a series of entries

Example of how to conform the `TimelineProvider` protocol

![/WWDC2020/images/Meet-WidgetKit/Untitled%2029.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2029.png)

Baked into each timeline is a ReloadPolicy. It's where you can tell the system when to ask for the next timeline.

### ReloadPolicy

- `atEnd`
- `after(date: Date)`
- `never`

### System reloads

- ReloadPolicy
The system will take into account your ReloadPolicy and determine the best time to reload your widget.
- Widgets viewed frequently receive more reloads
- Environment changes
The system will also force reloads for wherever a device environment changes.

### App-driven reloads

![/WWDC2020/images/Meet-WidgetKit/Untitled%2030.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2030.png)

The system will determine the best time to reload your widget but there are also other events which may need you to ask the system for a reload from your app.

For example, your app may receive a background notification, or a user may make a change in the app itself.

![/WWDC2020/images/Meet-WidgetKit/Untitled%2031.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2031.png)

When receiving a background notification, you can user WidgetKit API via WidgetCenter to reload your timeline, which will wake your extension. And if your user makes a relevant change in your foreground app,  you may also reload your timeline.

Be judicious with your foreground app reloads. Only reload your widget  when a relevant change in the app has occurred which should reflected in you widget.

You can use WidgetCenter APIs from within your app process or extension to reload your timeline.

### WidgetCenter

- `reloadTimelines(ofKind:)`
- `reloadAllTimelines`
- `getCurrentConfigurations(completion:)`

### URLSession

- Background sessions
- onBackgroundURLSessionEvents
- Batch requests to your server

### Reloads

- Background reloads are budgeted
- Be efficient with how much processing and networking your widget requires.

![/WWDC2020/images/Meet-WidgetKit/Untitled%2032.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2032.png)

There are many ways to drive reloads to help keep your widget up-to-date. Think through the right experience for your kind of widget. And keep in mind how to be efficient  with different ways to reload your timeline.

## Personalization and intelligence

- Intents, which are used as a mechanism to allow a user to configure a widget
- Relevance, which allows the developer to inform the intelligence in the stack.

### How WidgetKit works

- Intent frameworks
Intents are all powered by the Intents framework.
- Parameters
Intents contains a set of parameters which are questions to ask the user.
(ex. Weather's configuration question is the location for which to return a forecast)
- Used with Siri, Shortcuts, and now widgets

![/WWDC2020/images/Meet-WidgetKit/Untitled%2033.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2033.png)

The Stocks single symbol widget asks someone which stock to show. When the user tires to configure the widget, Intents can help answer this question by allowing Stocks to return the same list of stocks that are presented in the Stocks app. 

![/WWDC2020/images/Meet-WidgetKit/Untitled%2034.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2034.png)

What if someone wanted to show a stock that didn't already exist in the main app?

Thanks to the power of Intents, we can actually drive this type of experience using the Intents dynamic options capability. So the user can search in the configuration UI and the system will fire up the Stocks Intents extension which can then return series of answers in the form of stocks symbols.

![/WWDC2020/images/Meet-WidgetKit/Untitled%2035.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2035.png)

🆕New in iOS 14, Intents now support in-app Intent handling, where your app can answer these questions.

> What's New in SiriKit and Shortcuts - WWDC20

![/WWDC2020/images/Meet-WidgetKit/Untitled%2036.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2036.png)

Now we specify an `IntentConfiguration` instead of a `StaticConfiguration` and we specify an associated Intent.

![/WWDC2020/images/Meet-WidgetKit/Untitled%2037.png](/WWDC2020/images/Meet-WidgetKit/Untitled%2037.png)

Now evolves into the Provider conforming to the `IntentTimelineProvider`.

You will be passed in an Intent object, and based on the parameters in the Intent, you can generate a specific timeline.

The system can intelligently rotate to the most relevant widget and your app and widget can help feed this intelligence.

### Intelligence

- Shortcuts donation
When users perform actions in your app, your app can donate shortcuts. It your widget is backed by the same INIntent, then your widget may be rotated to in the stack when the user would have typically perform the action.

> Add Configuration and Intelligence to Your Widgets - WWDC20

- TimelineEntryRelevance
Widget extension also has the ability to annotate timeline entries with relevance values.

### TimelineEntryRelevance

- score
float value that you provide relative to all entries you have ever provided
- duration
time interval for when the particular entry is relevant
- Relative to all entries you have ever provided

When the time is appropriate and you feel your entry is the most relevant, then you may return a score and duration to inform the system to rotate to your particular widget.

## Wrap up

- Widgets are not mini-apps
- Glanceable
- Timelines, reloads, and intelligence
