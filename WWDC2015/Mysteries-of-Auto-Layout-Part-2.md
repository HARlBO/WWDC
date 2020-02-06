# Mysteries of Auto Layout, Part 2

> 📅 2020.2.6 (THU)
>
> WWDC 2015 | Session : 219 | Category : UI Frameworks  

🔗 [Mysteries of Auto Layout, Part 2 - WWDC 2015 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2015/219/)  

## The Layout Cycle

> Mystery #7

### Inside the Black Box

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-2/Untitled.png)

### The Layout Cycle

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-2/Untitled1.png)
High-level overview of the process

1. Application run loop cheerfully iterating 
2. until the constraints change in such a way  that the calculated layout needs to be different.
3. This causes a deferred layout pass to be scheduled.

When that layout pass eventually comes around, we go through the hierarchy and update all the frames for the views.

### Constraint Changes

**Changes to constraint expressions**

- Activating or deactivating
- Setting the constant or priority
- Adding or removing views

**Engine recomputes the layout**

- Engine variables receive new values
These expressions are made up of variables that represent things like the origin or the size of a particular  view.
- Views call `superview.setNeedsLayout()`

### Deferred Layout Pass

- Reposition misplaced views
- Two passes through the view hierarchy
    - Update constraints
    - Reassign view frames

> **updateConstraints**

Request via `setNeedsConstraints()`

→ Then some time later your update constraints method will be called.

Really, all this is a way for views to have a chance to make changes to constraints just in time for the next layout pass, but it's often not actually needed.

Often not needed

- Initial constraints in IB
All of your initial constraint setup should ideally happen inside Interface Builder.
Or if you really find that you need to allocate your constraints programmatically, some place like `viewDidLoad` is much better.
- Separate logic is harder to follow

Implement it when

- Changing constraints in place is too slow
It turns out that changing a constraint inside updateConstraint is actually faster than changing a constraint at other times. Because the engine is able to treat all the constraint changes that happen in this pass as a batch. This is the same kind of performance benefit that you get by calling activate constraints on an entire array of constraints as opposed to activating each of those constraints individually.
- A view is making redundant changes
It's efficient to have the view just call `setNeedsUpdateConstraints` and then when the update constraints pass comes along, it can rebuild its constraints once to match whatever the configuration is.

> **layoutSubviews** aka **layout**

Traverse the view hierarchy, top-down

- Call `layoutSubviews()` (or `layout()` on OS X)
The purpose is for the receiver to reposition its **subviews**.

Position the view's subviews

- Copy subview frames from the layout engine

Override `layoutSubviews()` for custom layout

- ...but be careful!

> Overriding **layoutSubviews**

Override when constraint are insufficient

Some views have already been laid out

Do

- Invoke `super.layoutSubviews()`
- Invalidate layout within your subtree
you should do that before you call through to the superclass implementation.

Don't

- Call `setNeedsUpdateConstraints()`
There was an update constraints pass. We missed it. If we still need it now, it's too late.
- Invalidate layout outside your subtree
It can be very easy to cause layout feedback loops where the act of performing layout actully causes the layout to be dirtied again.
- Modify constraints indiscriminately

**Remember**

- Don't expert frames to change immediately
- Proceed with caution when overriding `layoutSubviews()`

## Interacting with Legacy Layout

> Mystery #8

Positioning by frame versus constraints

Sometimes you need to set the frame

- e.g., if you're overriding `layoutSubviews()`  

```Swift 
    var translatesAutoresizingMaskIntoConstraints: Bool
```  
### translatesAutoresizingMaskIntoConstraints

Setting the frame automatically generates constraints 

→ that enforce that frame in the Layout Engine

- Set the frame with gleeful abandon!
- Constraints implement the autoresizingMask
- Other views can be constrained to it

Set to false when using constraints

- Beware-defaults to true for programmatically created views

## Constraint Creation

> Mystery #9

### Layout Constraint Creation

> 🆕 Layout anchors

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-2/Untitled2.png)

## Constraining Negative Space

> Mystery #10

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-2/Untitled3.png)

### 🆕 NSLayoutGuide / UILayoutGuide

- `UILayoutGuide` represents a rctangle in the layout engine
- Contrain just like a view
```Swift
    let guide = UILayoutGuide()
    view.addLayoutGuide(guide)
```  
- Layout anchors are not available for margins
- UIView now exposes `layoutMarginsGuide`
→ represents area of the view inside the margins.
```Swift
    var layoutMarginsGuide: UILayoutGuide
```  
# Debugging Your Layout

> Mysteries of Auto Layout, part 2

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-2/Untitled4.png)

## Unsatisfable Constraints

> Mystery #11

### Unsderstanding the Log

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-2/Untitled5.png)

**Adding identifiers**

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-2/Untitled6.png)

**Tips**

- Set accessibility identifiers
    - Identifies vies in logs
- Set identifiers on layout guides
- Add as you go
- View one axis at a time
    - `constraintsAffectingLayoutForAxis:` on iOS
    - `constraintAffectingLayoutForOrientation:` on OS X

### Understanding the Log

- Start from the bottom
- Check `translatesAutoresizingMaskIntoConstraints`
- Set identifiers
- Use `constraintsAffetctingLayoutForAxis:`

## Resolving Ambiguity

> Mystery #12

Why doen't my layout look right?

→ Possible causes

- Too few constraints
- Conflicting priorities

**Diagnostic tools**

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-2/Untitled7.png)

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-2/Untitled8.png)

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-2/Untitled9.png)

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-2/Untitled10.png)

![](/WWDC2015/images/Mysteries-of-Auto-Layout-Part-2/Untitled11.png)


### Debugging Your Layout

Think about what information the engine needs

Use the logs when constarints are unsatisfiable

- Add identifiers for constraints and views

Check for ambiguity regularly

Use tools to help resolve issues

- Icons in Interface Builder
- View debugger
- Methods in lldb
