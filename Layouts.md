# Layout :rocket

In ios development “layout” generally refers to the arrangement and positioning of different views. Layout defines how different UI elements such as text view, image view, button etc are displayed in the screen. 

---

## Auto Layout :boom

Auto Layout is the primary layout system used in iOS development. It is a constraint-based layout system that allows developers to create a responsive and adaptable user interface that can adjust to different screen sizes and orientations. Auto Layout is a concept which the size and position of all the views in the view hierarchy are dynamically calculated on run time, based on constraints placed on those views.

Constraints are the rule that defines the size and position of UI components with respect to each other or the parent view. Constraints are used to build responsive and addaptive design. 

Apple’s recommended way of defining layouts for most UIs is to use Auto Layout. Originally introduced all the way back in iOS 6 — at a time when we only had a small number of screen sizes to support — Auto Layout has undergone quite a lot of changes and improvements over the years, in particular with the introduction of layout anchors in iOS 9. When building layouts using Auto Layout, we no longer manually calculate the positions and sizes of our views — and instead use a constraint-based API to have the system perform those calculations on our behalf, by evaluating the constraints we have defined. Since this is done automatically (hence the name) whenever a layout condition — such as the rotation of the device — changes, supporting all the various devices iOS and macOS runs on becomes much easier.
Since constraints are both very powerful and complex, the “raw” API for dealing with them is quite verbose and somewhat cumbersome to work with. For example, here’s how we’d make a label’s height match the height of a button, by defining a constraint ourselves:

```swift

let constraint = NSLayoutConstraint(
    item: label,
    attribute: .height,
    relatedBy: .equal,
    toItem: button,
    attribute: .height,
    multiplier: 1,
    constant: 0
)


```

Layout anchors make the above kind of task a lot easier. Each UIView on iOS and NSView on macOS contains a series of anchors that can be used to automatically create constraints relative to other views. For example, here’s how the above constraint can be defined by using the heightAnchor of both views:

```swift
let constraint = label.heightAnchor.constraint(
    equalTo: button.heightAnchor
)

```

However, just like manually created constraints, the constraints created using layout anchors also need to be activated in order to be evaluated by the Auto Layout engine. That can either be done by setting the isActive property to true, or by using the activate API on NSLayoutConstraint to activate an array of constraints in one go:

```swift 

// Both of these are valid ways to activate a constraint
constraint.isActive = true
NSLayoutConstraint.activate([constraint])

```

The beauty of Auto Layout is that it’s not only capable of making simple 1:1 relationships between metrics, but can be used to create really complex layouts as well. For example, here we position our button from before at the center of its parent view — while also giving our label a minimum width and placing it beneath the button:

```swift

NSLayoutConstraint.activate([
 // Place the button at the center of its parent
 button.centerXAnchor.constraint(equalTo: parent.centerXAnchor),
 button.centerYAnchor.constraint(equalTo: parent.centerYAnchor),
// Give the label a minimum width based on the button’s width
label.widthAnchor.constraint(greaterThanOrEqualTo:button.widthAnchor),
// Place the label 20 points beneath the button
label.topAnchor.constraint(equalTo: button.bottomAnchor, constant: 20),
label.centerXAnchor.constraint(equalTo: button.centerXAnchor)
])


```
While layout anchors have vastly improved the user friendliness of Auto Layout, it’s still quite common to run into a few issues — especially in the beginning. So if your layout doesn’t initially look as you’d expect, here’s a few things to look out for:
<ul>
	<li> By default, all views automatically translate their initial auto resizing masks into layout constraints — which can conflict with the constraints we’ve defined in our code. To disable this behavior, simply set translatesAutoresizingMaskIntoConstraints to false on the view in question.  </li>
	<li> In order to define constraints based on anchors from multiple views, all views involved need to be a part of the same view hierarchy (otherwise an exception will be thrown at runtime). An easy way to assure that is to have all views that share the same layout code also share the same superview.  </li>
    <li> A constraint can be dynamically enabled and disabled — for example to adapt to changes like rotation or different UI states — by toggling that constraint’s isActive property. In general, it’s more performant to activate/deactivate a constraint rather than adding/removing it from its view. </li>

</ul>



## Frame Based Layout :fire

There is a another way of defining layout is frame based layout. Frame based layout system use coordinate system. The top left corner of the screen is the (x,y) → (0,0) point in frame based layout system. The coordinate system is shown in the following figure.

![phone1](https://github.com/shameem17/iOS-Tutorial/assets/53037559/fea3a8a1-479e-48ad-825b-f3497a755c91)


The end point of the coordinate system is the bottom right corner. We can create a view at x point 80.0, y point 244.0 with a width and height as below: 


```swift
let blueView = UIView(frame: CGRect(x: 80.0, y: 244.0, width: 160.0, height: 80.0))
blueView.backgroundColor = UIColor.blue
self.view.addSubview(blueView)

```

This will put a blue rectangle in X position of 80 (from left), Y position of 244 (from top) and a width of 160 and height of 80. Like this :

![phone2](https://github.com/shameem17/iOS-Tutorial/assets/53037559/03c10638-8d32-4490-92b4-728881f56bd9)

In this particular scenario the view seems to be placed accurately to the center of the screen. But when the phone changes ie. from iphone SE to iphone 8 then the position of the view will not be centered. To solve this problem Auto layout can be used. Another way is to use height, width of the parent view. Use parent view’s left, right, width, height, top, bottom to accurately calculate the position of using frame based layout. 

According to some expert’s opinion Auto Layout saves time and gives the guarantee in productivity, maintenance. Both auto layout and frame based layout are supported and have their own benefits. But mixing auto layout and frame based layout can cause a runtime error. 

References:

[Swift By Sundell](https://www.swiftbysundell.com/basics/layout-anchors/)

[Frame vs Auto Layout](https://fluffy.es/frame-vs-autolayout/)

[Stackoverflow](https://stackoverflow.com/questions/33850196/auto-layout-vs-frame-sizes)

[iOS Documentation](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)

[Learn auto layout](https://www.kodeco.com/811496-auto-layout-tutorial-in-ios-getting-started?page=1#toc-anchor-001)





