# Anatomy of an iOS app

In this lab we'll cover the basic building blocks of iOS and best practices for structuring your app. We'll discuss how **View Controllers** and **Views** are the foundation of iOS apps.

## Human Interface guidelines

Apple’s [Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/overview) is the best place to get a feel for the visual elements provided by [UIKit](https://developer.apple.com/documentation/uikit) - Apple’s framework for constructing iOS user interfaces. Perhaps more importantly, the guidelines provide an overview of Apple’s User Experience (UX) design philosophy as well as recommended best practices for the design of iOS apps. Pretty much every aspect of iOS interface design is covered conceptually:

![alt text][guidelines-sitemap]

You should spend some time studying the elements and patterns presented on the [site](https://developer.apple.com/ios/human-interface-guidelines/overview).

## Design resources

Once you familiarize yourself with the interface building blocks available in iOS, there are a variety of resources you can use to start designing your apps.

Some of the most popular apps for UX design and wireframing as of Jan 2018 include:

* [Adobe XD](https://www.adobe.com/products/xd.html)
* [Adobe InDesign](https://www.adobe.com/products/indesign.html), [Adobe Illustrator](https://www.adobe.com/products/illustrator.html), and [Adobe Photoshop](https://www.adobe.com/products/photoshop.html)
* [Sketch](https://www.sketchapp.com/)
* [Origami Studio by Facebook](https://origami.design/)
* [Invision](https://www.invisionapp.com/)
* [UXPin](https://www.uxpin.com/)

If you’re new to UX design and wireframing, the point of these apps is to help you layout your user interfaces, establish the flow of your app, and design your app's look and feel.

![alt text][adobe-xd-screenshot]

In addition to these applications, UI templates and kits are another useful resource. UI kits provide images of iOS user interface elements that you can copy and paste into your design tool of choice.

![alt text][wireframing-components]

A good number of template resources are free:

* Apple provides Photoshop, Sketch, and Adobe XD templates with the full set of iOS UI elements [here](https://developer.apple.com/design/resources/).

* Sketch provides a free iOS UI kit called [Elements](https://www.sketchapp.com/elements).

* Facebook provides iOS11 GUI templates for Origami, Sketch, and Photoshop [here](http://facebook.design/ios11).

With all of these awesome tools, we’d like to emphasize that what matters most is developing your ideas in a medium in which you feel comfortable - comfortable designing and comfortable communicating your ideas to others. If you already know Photoshop, you should use Photoshop for your wireframes. If you don’t know any of these tools, you should start with good ol’ pen and paper. Out in the Real World you won’t find a consensus from people as to what is the best UX software, even among professional designers. That said, Sketch and Adobe XD seem to be quite popular at the moment in addition to the time-tested Adobe trifecta (Photoshop, Illustrator, InDesign).

## Views

Views are the fundamental building blocks of your app's user interface. Every user interface element in iOS - from a button to an [Augmented Reality scene](https://developer.apple.com/documentation/arkit/arscnview) - is a subclass of [UIView](https://developer.apple.com/documentation/uikit/uiview). UIView defines the behaviors that are common to all views.

![alt-text][uiviews-subclasses]

Apple describes some of a View's responsibilities as:

> * **Drawing and animation**
>   * Views draw content in their rectangular area using UIKit or Core Graphics.
>   * Some view properties can be animated to new values.
>
> * **Layout and subview management**
>   * Views may contain zero or more subviews.
>   * Views can adjust the size and position of their subviews.
>   * Use Auto Layout to define the rules for resizing and repositioning your views in response to changes in the view hierarchy.
>
> * **Event handling**
>   * A view is a subclass of UIResponder and can respond to touches and other types of events.
>   * Views can install gesture recognizers to handle common gestures.

## View Controllers

You may remember seeing something named ViewController (UIViewController) in our first two labs [Hello Xcode](https://www.mobilelabclass.com/labs/hello-xcode.html) and [One Button Hookup](https://www.mobilelabclass.com/labs/one-button-hookup.html).

It turns out that **view controllers are the foundation of every iOS app.** In Apple's words:

> View controllers are at the center of almost everything you do.

If you open up your Xcode project for either of the first two labs, you'll see two files in the **Project Navigator** that Xcode created automatically for us when we selected the **Single View Application** template.

1. **Main.storyboard**
1. **ViewController.swift**

**Main.storyboard** is the storyboard file that you were introduced to in the first two labs. Opening a storyboard file in Xcode brings up **Interface Builder** where you can layout your user interface.

If you click on the other file that Xcode created for us - **ViewController.swift** - you will see some swift source code:

```swift
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
}
```

As you can see, our ViewController [inherits](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html) from [UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller).

**UIViewControllers are the engines of your app**. They facilitate the overall flow of your application by instantiating and presenting views, managing transitions, handling when the app goes to the background, and responding to user interaction, etc.

Apple's documentation describes a view controller’s main responsibilities as:

> * Updating the contents of the views, usually in response to changes to the underlying data.
>
> * Responding to user interactions with views.
>
> * Resizing views and managing the layout of the overall interface.
>
> * Coordinating with other objects—including other view controllers—in your app.

You can see some of these responsibilities in the source code for our ViewController.swift:

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }
```

The **viewDidLoad()** method is a hook into your View’s lifecycle - the process for loading and unloading the user interface of your application. The system calls this method when your app has instantiated (loaded) your View.

Similarly, the **didReceiveMemoryWarning()** method is called by the system when your app is consuming too many resources. We don’t need to worry about this at the moment but the gist of it is that the method is called when your device is running out of RAM. The system sends this message to request that you free up any unnecessary resources that your app may be using.

For all you [Processing](processing.org) gurus, you can think of these methods as similar to functions like **void setup()** in Processing - a way to tap into important events related to your app.

While the structure of a Processing sketch looks something like this:

```processing
    void setup() {
        // Called once when the program starts
        // Define initial enviroment properties such as screen size
    }

    void draw() {
        // Continuously execute the lines of code contained inside this block
    }

    void mousePressed() {
      // Called once after every time a mouse button is pressed.
    }
```

The structure of an iOS view controller might look like this:

```swift
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Sorta like the setup() function in processing
        // Let’s setup a button color
        myButton.backgroundColor = UIColor.red
    }

    @IBAction func handleButton(_ sender: UIButton) {
        // Called once each time a button is tapped
    }
}
```

Note the similarities between **viewDidLoad()** in iOS and **setup()** in Processing. Also note the similarities between **handleButton()** in iOS and **mousePressed()** in Processing. All of these methods provide opportunities for you to respond to system-initiated and user-initiated events.

In a single view application with only one view controller, you might even think of the view controller as the iOS version of a  Processing sketch - it's where you'll write alot of your application logic, decide what’s drawn on screen, and react to user interactions like taps and gestures. Unlike a Processing sketch however, all but the simplest apps will contain multiple View Controllers with lots of methods in each.

One more thing to note is the [override prefix](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html) in the **viewDidLoad** method definition:

```swift
override func viewDidLoad() {
  super.viewDidLoad()
}
```

This indicates that we are overriding the default implementation **viewDidLoad()** from the base UIViewController class. Our custom ViewController [inherits](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html) from the base [UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller).

 In simpler terms, overriding allows us to customize the behavior of our app while tapping into some functionality that Apple has already created for us. In **viewDidLoad()**, we are given an opportunity to do our own custom setup after our view has been loaded - like the **setup()** function in Processing.

## Views on Views on Views . . . on View Controllers

Understanding the relationship between View Controllers and Views is fundamental to understanding how to put together iOS apps.

From the Apple [docs](https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457-CH2-SW1):

> The most important role of a view controller is to manage a hierarchy of views. Every view controller has a single root view that encloses all of the view controller’s content. To that root view, you add the views you need to display your content.

![alt-text][view-controller-view-hierarchy]

Any screen can be decomposed into a graph of View Controllers and Views. In the following image, we graph a likely architecture of an Instagram profile:

![alt-text][itp-instagram-view-controller-scene]

## View Controllers in XCode

Going back to the first labs, you'll notice in Xcode that the project template automatically connected our ViewController defined in **ViewController.swift** with a ViewController in Interface Builder. You can see this in the far right side of Xcode under the **Custom Class** section of the identity inspector.

![alt-text][view-controller-assignment]

You'll also notice that our View Controller does indeed contain a root view - this is the blank view that we dragged our label onto in the first lab. Like all UI elements, labels are a subclass of **UIView**.

![alt-text][view-hierarchy]

When creating new View Controllers, Xcode helps us out by stubbing out some of the method definitions necessary to make them run. This is similar to the way that the Processing IDE will automatically include the required **void setup()** and **void draw** functions when we start a new Processing sketch.

For example, when we create a new **UITableViewController** in Xcode (the view controller responsible for managing tables), Xcode will include the following methods necessary for a table to function:

```swift
override func numberOfSections(in tableView: UITableView) -> Int {
  // Return the number of sections
  return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // Return the number of rows
  return 99
}

override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
  let cell = tableView.dequeueReusableCell(withIdentifier: "reuseIdentifier", for: indexPath)

  /// Configure the cell...

  return cell
}
```

Don't worry for now about the nitty gritty of the above code. The important things to remember are:

1. ViewControllers implement specific methods related to the specific functionality we want in our views.
2. Xcode helps us when we create a new ViewController by including some of the methods necessary to implement basic UI elements (like Table Views, Collection Views, Augmented Reality Views, Scroll Views, etc).

Apple provides many default Views and View Controllers across all of its frameworks to help you implement your ideas:

| Framework        | View Controllers |
| ------------- |--------------|
| UIKit     | View controllers for displaying alerts, taking pictures and video, and managing files on iCloud. UIKit also defines many standard container view controllers that you can use to organize your content. |
| GameKit      | View controllers for matching players and for managing leaderboards, achievements, and other game features.      |
| Address Book UI | View controllers for displaying and picking contact information.      |
| MediaPlayer | View controllers for playing and managing video, and for choosing media assets from the user’s library.      |
| EventKit UI | View controllers for displaying and editing the user’s calendar data.     |
| GLKit | View controller for managing an OpenGL rendering surface.   |
| Social | View controllers for composing messages for Twitter, Facebook, and other social media sites.  |
| AVFoundation | View controller for displaying media assets. |

In the next few labs we'll roll up our sleeves and dive into the details of how to work with View Controllers. Over the course of this class we will cover many of most popular controllers across iOS 11.

<!--
  Image references
-->

[guidelines-sitemap]: https://mobilelaboratory.s3.amazonaws.com/anatomy/guidelines-sitemap.jpg "Sitemap for Apple's Human Interface Guidelines"

[adobe-xd-screenshot]:https://mobilelaboratory.s3.amazonaws.com/anatomy/AdobeXD_Design.jpg "Adobe XD"

[xcode-screenshot-viewcontroller-view]:https://mobilelaboratory.s3.amazonaws.com/anatomy/single-view-template-mvc.jpg "Xcode screenshot of view controller and view"

[view-controller-view-hierarchy]:https://mobilelaboratory.s3.amazonaws.com/anatomy/viewcontroller-view-hierarchy.png "Xcode screenshot of view controller and view hierarchy"

[view-controller-assignment]:https://mobilelaboratory.s3.amazonaws.com/anatomy/viewcontroller-class.jpg "View controller class assignment"

[view-hierarchy]:https://mobilelaboratory.s3.amazonaws.com/anatomy/view-hierarchy.jpg "View hierarchy"

[decompose-view-controller-scene]:https://mobilelaboratory.s3.amazonaws.com/anatomy/decompose.png "Decomposed view controller scene"

[itp-instagram-view-controller-scene]:https://mobilelaboratory.s3.amazonaws.com/anatomy/instagram-anatomy.jpg "ITP Instagram Profile view controller scene"

[wireframing-components]:https://mobilelaboratory.s3.amazonaws.com/anatomy/wireframing-components.jpg "Wireframing components"

[uiviews-subclasses]:https://mobilelaboratory.s3.amazonaws.com/anatomy/views.jpg "UIView subclasses"
