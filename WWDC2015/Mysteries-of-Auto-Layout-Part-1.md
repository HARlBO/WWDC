# Mysteries of Auto Layout, Part 1

>  📅 2020.2.4 (TUE)
>
> WWDC 2015 | Session : 218 | Category : UIKit

🔗 [Mysteries of Auto Layout, Part 1 - WWDC 2015 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2015/218/)

## Maintainable Layouts

> Mystery #1

### Constraints

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled.png)

### Stack View

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled1.png)

- Built  with Auto Layout
- Manages constraints
- Horizontal or vertical

### Alignment

- Horizontal StackView

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled2.png)

- Vertical StackView

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled3.png)

### Distribution

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled4.png)

This allows you to distribute along the axis, and you can do some pretty complex distribution behaviors without having to deal with any constraints.

- Fill : filling the stack view.
- Fill equally, Fill Proportionally : based on content size of the subviews
- Equal Spacing

### Animate

Animation is really easy on stack view
```Swift
    UIView.animate(withDuration: 1) {
    	self.subviewToHide.hidden = !self.subviewToHidden.hidden
    }
```

### API

It's simple, familiar, straightforward .

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled5.png)

`arrangedSubviews` :

This property returns you a subset of all of the views that are owned by the stack view. It returns you the view that are currently being stacked.

What that implies is that you can have views that are not being stacked by StackView, such as decorators or overlay, and have a clean view hierarchy, and we think you will find that useful.

### Stack View in Interface Builder

- Easy to build
- Easy to maintain
- Composable Stack Views
- Lightweight

→ Stack view is about layout and therefore it doesn't need to render its own background, we've been able to do some optimizations. So we have a special Transform Layer class that doesn't render on its own that can make Stack View run fast and even more performant than your regular views.

### Before and After

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled6.png)

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled7.png)

## Feeding the Layout Engine

> Mysteries of Auto Layout, part 1

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled8.png)

## Changing Constraints

> Mystery #2

### Activate and Deactivate

- Constraints find their own container
- Adds constraints efficiently
- Do not need to own all views

❌ Add and remove

✅ Activate and deactivate

### Things to Keep in Mind

🙅🏻‍♀️**Never deactivate** `self.view.constraints`

- Not all of those constraints in that array belong to you
(such as the ones the view uses to set itself up)
- Weird things will happen
- Just don't do it!

🙆🏻‍♀️ Keep references to constraints that change

(either in an array of constraints or as individual constraints so that you can manage things exactly the way you want to.)

### Changing Constraints

- Never deactivate `self.view.constraints`
- Keep references to constraints
It will make everything work better, and you can do cool thing like animation.
- Animate changing constraints with view animation

## View Sizing

> Mystery #3

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled9.png)

**Defining a particular view size**

- Use constraints first
- Override `intrinsicContentSize` for specific reasons
    - If size information does not come from constraints
    - If view has custom drawing (sometimes)
    - You will be responsible for invalidating
    in case the slide class changes or the text changes that you've got dynamic text, localization,   or anything like that may cause your view to need to recalculate its internal size.
- Size can change with size class changes

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled10.png)

The more you rely on proportions, the more likely you are to have a good layout that will be easy for you to put across all these different screens we are giving you.

### Self-Sizing Table View Cells

> Mystery #4

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled11.png)

### View Sizing

- Certain views have an `intrinsicContentSize`
You can use that when you are defining relationships to other views.
- Constraints should define size when possible
When not, you can override  `intrinsicContentSize`, but make sure you invalidate
- For self-sizing views, define size fully in constraints

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled12.png)

## Priorities

> Mystery #5

### Ambiguity

If you have been doing Auto Layout for a while, you may have landed in a situation where views aren't landing where you expect them to in regard their superview.

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled13.png)

### Constraint Priorities

So how do priorities work?

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled14.png)

### Constraint Priorities

- How a view handles its content
- By default, these are not set as required
    - Do not set as required
    - Can cause unsatisfiable constraints
- Equal priorities can cause ambiguity
- Types
    - Content hugging
    - Compression resistance

**Content hugging**

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled15.png)

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled16.png)

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled17.png)

TextView 249

→ The engine looks at your constraints and your priorities and say, oh the text view hugging priority is fairly important, but the button hugging priority is not at all important, so I can go ahead and stretch that away from its content to fill up this horizontal part of your view here.

TextView 251

→ The engine says, hey now I want to hug this button text really closely, and that means that I can stretch the text field in order to solve your layout.

**Compression resistance**

This is is how much a view resists its content getting squished.

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled18.png)

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled19.png)

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled20.png)

What if you localize this app?

I can either shrink the imageView or clip label.

→ If you wanted the label to always show all of its content and it was okay to shrink that image a little bit, all you have to do is set the labels content compression resistance priority slightly higher than that of the imageView.

### Priorities

- Can help keep constraints from unsatisfiability
    - But look out for competing priorities!
- Results are more consistent
- Use content priorities to get to the right layout
    - Hugging priorities hug content
    - Compression resistance resist squishing

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled21.png)

## Alignment

> Mystery #6

- Use `firstBaseline` and `lastBaseline`
- Aligns text better than top or bottom
- Better control over changing views

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled22.png)

the button can sort of end up in a nebulous area.

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled23.png)
align by last baseline

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled24.png)

align by first baseline

### Leading and Trailing

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled25.png)

### Alignment Rects

What the engine actually calculates. The engine takes all of your constraints, calculates the alignment rects, and uses them to actually layout views.

- Usually (not always) same as frame
- Includes the critical content only
- Does not change when view is transformed
- Override `alignmentRectInsets` if needed
- Find out the calculated rects
    - Use Show Alignment Rectangles in Debug menu
    - Get using `alignmentRectForFrame:`

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled26.png)

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled27.png)

### Alignment

- First and last baseline for better aligned text
- Leading and trailing instead of left and right
- Override `alignmentRectInsets` to adjust alignment rects

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled28.png)

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-1/Untitled29.png)


## Summary

- Stack Views help build easily maintainable layouts
- Use activate and deactivate for constraints
- Determine size through constraints
    - Override `intrinsicContentSize` judiciously
- Use priorities to properly solve your layout
- Alignment goes beyond top, bottom, and center
    - Keep localization in mind
