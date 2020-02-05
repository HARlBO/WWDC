# Cocoa Touch Best Practices

> 📅 2020.1.30 (THU)

WWDC 2015 | Session : 231| Category : UIKit

🔗 [Cocoa Touch Best Practices - WWDC 2015 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2015/231/)

![](/WWDC2015/images/Cocoa-Touch-Best-Practices/Untitled.png)

## App Lifecycle

First best practice is launch quickly. 
That's  how you appear responsive when the user taps on your icon and right away they get your app ready to interact with.

### Launch Quickly

- Return quickly from `applicationDidFinishLaunching`
    - Defer long running work
    - App killed if too much time passes

### Beyond App Launch

> Being responsive to every input

- Not just about asynchrony
- Move long running work to background queues

```Swift
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]?) -> Bool {
    	globalDataStructure = MyCoolDataStructure()
    
    	// defer work until later
    	DispatchQueue.global().async {
    		globalDataStructure.fetchDataFromDatabase()
    	}
    
    	return true
    }
```

Whenever it does run, user interface continues to happen, and your application seems responsive all the time. So this technique, putting the work on background queues, can be applied anytime in your app.

### Launch Quickly Again

![](/WWDC2015/images/Cocoa-Touch-Best-Practices/Untitled1.png)

That app is the first on that's going to die when the foreground app needs additional memory.

⬇️

![](/WWDC2015/images/Cocoa-Touch-Best-Practices/Untitled2.png)

### Leverage Frameworks

> Properly manage version change

- Target two most recent major release
- Include version fallbacks
- Have an else clause

## Views and View Controllers

### Layout on Modern Devices

![](/WWDC2015/images/Cocoa-Touch-Best-Practices/Untitled3.png)

No longer makes sense to build layouts that are built specific for a particular dimension that your ViewController expects to be in.

Instead, layout in general wants to think of itself as being done to proportions, and we do that by specifically avoiding hard-coded values in the layout of Views and ViewControllers.

### Layout to Proportions

- Avoid hard-coded layout values
- Either or both dimensions may scale

![](/WWDC2015/images/Cocoa-Touch-Best-Practices/Untitled4.png)

![](/WWDC2015/images/Cocoa-Touch-Best-Practices/Untitled5.png)

### Size Classes

- Some size thresholds trigger major change
- Packaged in UITraitCollection

### Properties, No Tags!

Avoid `setTag:` and `viewWithTag:`

- Possible collisions with other code
- No compiler warnings
- No runtime errors

Instance variables and properties provide better alternative 

```Swift
    class ViewController: UIViewController {
    	var imageView: UIImageView
    
    	overrride func viewDidLoad() {
    		super.viewDidLoad()
    
    		imageView = UIView(frame: CGRect(x: 0, y :0, widht: 50, height: 50))
    		view.addSubview(imageView)
    	}
    }
```

Keep real reference to that view, also has better type information because `viewWithTag:`  only is a type UIView.

### Make Timing Deterministic

Leverage `UIViewControllerTransitionCoordinator`

(to know what the timings are for the animations that you have)

- Animate alongside a transition
- Get accurate completion timing
- Support interactive and cancelable animations

## Auto Layout

### Modify Constraints Efficiently

- Identify constraints that get changed, added, or removed
- Unchanged constraints are optimized
- Avoid removing all constraints
- Use explicit constraint references

❌❌❌
```Swift
    override func updateViewConstraints() {
    	super.updateViewConstraints()
    
    	view.removeConstraints(view.constraints())
    	view.addConstraints(self.genereatdConstraints())
    }
```

✅✅✅
```Swift
    override func updateViewConstraints() {
    	super.updateViewConstraints()
    
    	view.removeConstraints(imageViewHorizontalConstraint)
    	imageViewHorizontalConstraints = self.generatedImageViewHorizontalConstraint()
    	view.addConstraint(imageViewHorizontalConstraint)
    }
```

### Constraint Specificity

> De-duplicating constraints

- Duplicates are implied by existing constraints
- Duplicates causes excess work in layout engine
It's solving for these constraints because they are there, but didn't actually need to solve for them.

![](/WWDC2015/images/Cocoa-Touch-Best-Practices/Untitled6.png)

This view hierarchy would have laid out exactly the same if I hadn't specified the left margin of the bottom view.

Get rid of that we'll be faster.

> Create flexible constraints

- Avoid hard-coded values
- Describe constraints using bounds

![](/WWDC2015/images/Cocoa-Touch-Best-Practices/Untitled7.png)

![](/WWDC2015/images/Cocoa-Touch-Best-Practices/Untitled8.png)

> Fully specify constraints

- Underspecification generates ambiguity
- Ambiguity is undefined
View might come out different ways different times I run my app.

> Testing and debugging

-[UIView hasAmbiguousLayout]
It will let you know if there's ambiguity in you view.

- When called on UIWindow, returns result for entire view tree

-[UIView _autolayoutTrace]

- Both can be used for unit testing

## Table and Collection View

### Self-Sizing Cells

- Fully specify constraints
- Width = input, height = output

### Animation Height Changes

- `tableView.beginUpdates`
- Update model
- Update cell contents
- `tableView.endUpdates`

### Fast Collection View Layout Invalidation

> Modify only what is needed

1. Invalidate on bounds change
2. Build targeted invalidation context
3. Repeat as necessary
