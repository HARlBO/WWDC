# Design great widgets

> 📅 2020.7.2 (THU)
>
> 🗂 WWDC2020 | Session : 10103 | Category : Frameworks
>
> 🔗 [https://developer.apple.com/videos/play/wwdc2020/10103/](https://developer.apple.com/videos/play/wwdc2020/10103/)
>  <br />
>  <br />
> 🔖 Widgets elevate timely information from your app to primary locations on iPhone, iPad and Mac. Discover the keys to designing glanceable widgets, developing a strong widget idea, and clearly communicating with content, color, sizing, layout, and typography. If you'd like to learn more about the technical implementation for adding widgets into your app, check out "Get Started with WidgetKit" and our three-part code-along series.
> <br />
<br />  

![/WWDC2020/images/Design-great-widgets/Untitled.png](/WWDC2020/images/Design-great-widgets/Untitled.png)

In iOS 14, the entire widget has been completely redesigned. At its foundation is an entirely new visual aesthetic, and powerful new capabilities.

One of the things we're really excited about is that people can now add widgets directly onto their home screen pages.

![/WWDC2020/images/Design-great-widgets/Untitled%201.png](/WWDC2020/images/Design-great-widgets/Untitled%201.png)

Another feature is Smart Stacks. These let you add several different widgets into a single location and quickly flick between the different widgets inside.

But what is really powerful is that smart stacks dynamically change and adapt to how you use them. Based on your behavior and context, a smart stack will automatically rotate to show you the most relevant widget at a given time.

When designing a widget there are 2 main parts of process to focus on, **Ideation and Creation.**

# Ideation

## Principles

![/WWDC2020/images/Design-great-widgets/Untitled%202.png](/WWDC2020/images/Design-great-widgets/Untitled%202.png)

- **Personal** 
We want to look for things that are personal because they can allow for a deeper emotional connection with a piece of your app or an experience that it enables for someone
- **Informational**
Widgets provide a great way to see a top level overview of information from a variety of sources on someone's device. Surfacing the right information can save people from doing commonly repeated actions in your app.
- **Contextual**
Context helps surface the right information at the right moment and allows for better experience that at its best feels like it's magically prediction someone's need and next steps.

### Example

**Calendar** 

![/WWDC2020/images/Design-great-widgets/Untitled%203.png](/WWDC2020/images/Design-great-widgets/Untitled%203.png)

It shows the day of the week and the current date, your next meeting or event was the most important piece of information to surface from within the app. 

Glanceable details like the start time and event location save people a potential step in opening the app to find this information.

If you have a busy schedule with lots of events it collapses some of the less relevant information and prioritizes the single most important piece of information from each event.

![/WWDC2020/images/Design-great-widgets/Untitled%204.png](/WWDC2020/images/Design-great-widgets/Untitled%204.png)

When your day is almost ever and there's no more events, rather than just showing a blank widget when possible we instead start telling your about what is coming up next and happening in your day tomorrow.

Widgets are dynamic and informative, and contextual details make the experience feel very personal and adaptive to person's need.

## Editing

In iOS 14, your widgets will jiggle just like apps do in edit mode and you can tap on a widget to flip it around and see what it allows for you to edit.

![/WWDC2020/images/Design-great-widgets/Untitled%205.png](/WWDC2020/images/Design-great-widgets/Untitled%205.png)

Here, the Weather widget lets me change what location it's displaying the weather conditions for. By default it adapts to your current location so that when person adds a widget they don't have to do any additional work.

![/WWDC2020/images/Design-great-widgets/Untitled%206.png](/WWDC2020/images/Design-great-widgets/Untitled%206.png)

If I tap on the location field, I get a list of all my favorite weather cities as well as the ability to search for other locations.

![/WWDC2020/images/Design-great-widgets/Untitled%207.png](/WWDC2020/images/Design-great-widgets/Untitled%207.png)

Most of the new widgets in iOS support editing like this and it's a great experience to add and choose different reminders list, stocks, notes and world clocks.

## Multiples

![/WWDC2020/images/Design-great-widgets/Untitled%208.png](/WWDC2020/images/Design-great-widgets/Untitled%208.png)

For Stocks, we had two different ideas: to offer a widget that shows a glanceable compact summary of your watchlist information but also the ability to add a single stock symbol as a separate widget to keep track of it at a higher resolution just like weather did.

Several other apps in iOS 14 offer multiple widgets, including news which lets you follow top news from a specific topic and notes that lets you pin a favorite note or add a shared note folder.

# Creation

## Sizes and Interactions

![/WWDC2020/images/Design-great-widgets/Untitled%209.png](/WWDC2020/images/Design-great-widgets/Untitled%209.png)

### **Small**

- All about the most useful piece of content from your app in a size that constraints how much content can actually fit in it.
- Small widget supports a single tap  target. Tapping it should deep link to the content that's on the widget.

Ex) small Calendar widget

![/WWDC2020/images/Design-great-widgets/Untitled%2010.png](/WWDC2020/images/Design-great-widgets/Untitled%2010.png)

![/WWDC2020/images/Design-great-widgets/Untitled%2011.png](/WWDC2020/images/Design-great-widgets/Untitled%2011.png)

Since the widget is always showing what event is coming up next, we thought it made the most sense to bring you to the latest event in the app's Day View.

When you tap on the widget, the Day View scrolls to the latest event and gives you a nice glimpse of the rest of your day around it.

Ex) small News widget
|||
|-|-|
|![/WWDC2020/images/Design-great-widgets/Untitled%2012.png](/WWDC2020/images/Design-great-widgets/Untitled%2012.png)|![/WWDC2020/images/Design-great-widgets/Untitled%2013.png](/WWDC2020/images/Design-great-widgets/Untitled%2013.png)|

Since it previews a rich news story that you might be interested in reading, tapping it brings you directly to that news story and the app.

### Medium, Large

- Both sizes fit more content and support multiple tap targets. Tapping a piece of content in a medium or large widget should also deep link you to the displayed content that's on the widget

Ex) News
|||
|-|-|
|![/WWDC2020/images/Design-great-widgets/Untitled%2014.png](/WWDC2020/images/Design-great-widgets/Untitled%2014.png)|![/WWDC2020/images/Design-great-widgets/Untitled%2013.png](/WWDC2020/images/Design-great-widgets/Untitled%2013.png)|

Tapping either article brings you directly to the News story you tapped on. 
||||
|-|-|-|
|![/WWDC2020/images/Design-great-widgets/Untitled%2015.png](/WWDC2020/images/Design-great-widgets/Untitled%2015.png)|![/WWDC2020/images/Design-great-widgets/Untitled%2016.png](/WWDC2020/images/Design-great-widgets/Untitled%2016.png)|![/WWDC2020/images/Design-great-widgets/Untitled%2017.png](/WWDC2020/images/Design-great-widgets/Untitled%2017.png)|

- Fill style is best for when you're deep linking into a single piece of content. Every small widget uses fill style since it only supports one tap target.
- Cell style is best for when you're selecting a piece of content in a widget that lives in its own shape.
- Content style is great for when you're selecting a piece of content that lives un-contained in a widget.

## Content and Personality

The most important part about making your widget come to life is what lives in it. You should think about content and personality when you're designing your widget.

**" What are people looking for when they launch my app?"**

Also find distinct items of information that people find useful in your app.

![/WWDC2020/images/Design-great-widgets/Untitled%2018.png](/WWDC2020/images/Design-great-widgets/Untitled%2018.png)

When designing widget set, we looked at finding personality through how our apps look.

For Weather, we used the familiar weather condition background, as well as the iconography from the app.

![/WWDC2020/images/Design-great-widgets/Untitled%2019.png](/WWDC2020/images/Design-great-widgets/Untitled%2019.png)

For News, we took inspiration from the rich story images that you see in news stories.

![/WWDC2020/images/Design-great-widgets/Untitled%2020.png](/WWDC2020/images/Design-great-widgets/Untitled%2020.png)

And for Calendar, we took inspiration from its simplistic look and familiar red tint color, focusing all on what events are coming up next.

Another approach for finding personality is taking inspiration from your app icon.

![/WWDC2020/images/Design-great-widgets/Untitled%2021.png](/WWDC2020/images/Design-great-widgets/Untitled%2021.png)

Like Notes, where we took on the icons notepad illustration style.

![/WWDC2020/images/Design-great-widgets/Untitled%2022.png](/WWDC2020/images/Design-great-widgets/Untitled%2022.png)

Podcasts, where we use the icon's purple gradient.

![/WWDC2020/images/Design-great-widgets/Untitled%2023.png](/WWDC2020/images/Design-great-widgets/Untitled%2023.png)

And Tips where we use the icons yellow gradient.

When it come to laying out content in your widget, there were two patterns we found when designing our widget set.

1. Layout that expands across all three sizes.
like Weather, where we add additional information across each size.
2. Layout that is completely unique across each size.
like News, the small widget prioritizes rich content that fills up the whole space, 
while the medium widget focuses on showing more news stories.

![/WWDC2020/images/Design-great-widgets/Untitled%2024.png](/WWDC2020/images/Design-great-widgets/Untitled%2024.png)

When you're designing for each size, make sure not to scale up your smaller widget into your large widget.

Think about the information you're working with and what makes the most sense for each size. 

![/WWDC2020/images/Design-great-widgets/Untitled%2025.png](/WWDC2020/images/Design-great-widgets/Untitled%2025.png)

For the large Screen Time widget, since we had more useful information that we could include and more space, we increased the size of the chart and also included category and app details. 

If you don't have more information to show in your larger sizes though, it's fine to only support specific sizes of your idea. All sizes for an idea aren't required.

![/WWDC2020/images/Design-great-widgets/Untitled%2026.png](/WWDC2020/images/Design-great-widgets/Untitled%2026.png)

We recommend only including a max of four pieces of information in the small widget size.

## Patterns

![/WWDC2020/images/Design-great-widgets/Untitled%2027.png](/WWDC2020/images/Design-great-widgets/Untitled%2027.png)

Across the different small, medium and large sizes, there were several common layout patterns that emerged.

![/WWDC2020/images/Design-great-widgets/Untitled%2028.png](/WWDC2020/images/Design-great-widgets/Untitled%2028.png)

There are mix of single item and denser multi-item summary layouts. 

![/WWDC2020/images/Design-great-widgets/Untitled%2029.png](/WWDC2020/images/Design-great-widgets/Untitled%2029.png)

These patterns serves as a helpful starting place and a good way to try out an idea in a format that already works well.

![/WWDC2020/images/Design-great-widgets/Untitled%2030.png](/WWDC2020/images/Design-great-widgets/Untitled%2030.png)

When designing your own custom layouts, follow the default sixteen point layout margins across all sizes to make sure the content in your widget feels consistent when it's place next to other widgets.

![/WWDC2020/images/Design-great-widgets/Untitled%2031.png](/WWDC2020/images/Design-great-widgets/Untitled%2031.png)

For layout with graphical shapes like circles and inset platters use tighter eleven point margins across all sizes. 

![/WWDC2020/images/Design-great-widgets/Untitled%2032.png](/WWDC2020/images/Design-great-widgets/Untitled%2032.png)

Shape corners that sit close to the edges of your widget should appear concentric with the widget corner radius. 

Since the widgets corner radius changes across different device sizes, we provide a SwiftUI container that can assign to shapes in your widget that will make them concentric with the widgets corner radius values.

For type, you should use `SF Pro` or other variants of `San Francisco` that are available like `SF Mono` and `SF Pro Rounded`. If a custom font is important to how your widget represents its brand or personality, make sure it's applied in a way so that your widget still feels at home alongside other widgets. 

A widget should look great in both light and dark appearance modes. 

![/WWDC2020/images/Design-great-widgets/Untitled%2033.png](/WWDC2020/images/Design-great-widgets/Untitled%2033.png)

Every widget must provide a placeholder which is shown when the system has no way of displaying your widgets data.

You should show the base graphical elements in this state and block in areas of text where your information is shown in the layout.

![/WWDC2020/images/Design-great-widgets/Untitled%2034.png](/WWDC2020/images/Design-great-widgets/Untitled%2034.png)

This way, when the system goes form place holder to the proper data, the content can replace the static elements without having the layout or color shift around.

## Tips

![/WWDC2020/images/Design-great-widgets/Untitled%2035.png](/WWDC2020/images/Design-great-widgets/Untitled%2035.png)

You should only use logo in your widget if your app is an aggregator of content from different sources. 

![/WWDC2020/images/Design-great-widgets/Untitled%2036.png](/WWDC2020/images/Design-great-widgets/Untitled%2036.png)

To keep this treatment consistent across different widgets, your logo should always sit in the top right corner.

Avoid using word marks in the space and anywhere else in your widget.

![/WWDC2020/images/Design-great-widgets/Untitled%2037.png](/WWDC2020/images/Design-great-widgets/Untitled%2037.png)

Another thing to avoid is putting your app icon in your widget. 

![/WWDC2020/images/Design-great-widgets/Untitled%2038.png](/WWDC2020/images/Design-great-widgets/Untitled%2038.png)

Also avoid putting your app name in your widget as it will feel redundant with the app label that already appears underneath of it on the home screen.

![/WWDC2020/images/Design-great-widgets/Untitled%2039.png](/WWDC2020/images/Design-great-widgets/Untitled%2039.png)

Don't put text that instructs a user or talks to them, instead, if you feel there's something import to communicate do it in a graphical way.

![/WWDC2020/images/Design-great-widgets/Untitled%2040.png](/WWDC2020/images/Design-great-widgets/Untitled%2040.png)

When displaying chronological information on a widget, don't use language like "last updated" or "last checked".
