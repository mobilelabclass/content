# Hello Xcode

[Xcode](https://developer.apple.com/xcode/) is a software suite from Apple used to create apps for all Apple platforms (iOS, macOS, tvOS). It helps you write and compile code, lay out your user interface, and run your app on both a software simulator and on actual iOS hardware.

Download Xcode from the App Store [here](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) and install.

The easiest way to start a new iOS app is through the project templates provided by Xcode.

---

## Starting a new project

Open Xcode from the /Applications directory on your Mac.

Click Create a New Project from the Welcome screen or through the menu bar: **File > New > Project**

![alt text][create-project-from-welcome]

![alt text][create-project-from-menu]

Xcode will present you with a set of project templates to select from. Make sure that iOS is selected at the top of the menu and select **Single View App**. As the name implies, this is the starting point for an app with a single view (aka screen).

![alt text][single-view-app-template]

When you start a new project, you will be asked to choose from a handful of options such as the programming language you intend to write your app in. You will also be asked to provide some unique identifiers for your app.

In the screen that follows, enter **helloworld** under **Product Name**.

Under **Organization Name**, enter your name without any spaces *(for example, janedoe)*.

Finally, under **Organization Identifier**, enter **com.[your organization name]** *(for example, com.janedoe)*.The organization identifier and product name are automatically concatenated to create a bundle identifier - a unique id used to identify your app when you submit it to the App Store. For this exercise, the actual values you enter now do not matter too much. If you want to learn more about bundle identifiers, [Cocoacasts has a nice write-up here](https://cocoacasts.com/what-are-app-ids-and-bundle-identifiers), but it is not necessary for this exercise.

Make sure that **Swift** is selected under **language** and click Next.

![alt text][setting-up-a-new-project]

In the menu that follows, select a location to save your project.

On the bottom of the screen you will see a select box with the text: **Create Git repository on my Mac**. Make sure that this box is checked. We will discuss **git** in the next lab.

Congrats! You're ready to start your first iOS project!

---

## Using Interface Builder & Storyboards

As described in the [Apple docs](https://developer.apple.com/library/content/documentation/General/Conceptual/Devpedia-CocoaApp/Storyboard.html):
> A storyboard is a visual representation of the user interface of an iOS application, showing screens of content and the connections between those screens.

Xcode provides a visual editor called **Interface Builder** to help you modify your storyboards.

With your project open in Xcode, select **Main.storyboard** from the column on the left. Interface Builder will load the storyboard that Xcode created for you when you selected the project template.

![alt text][interface-builder-anatomy]

In this editing mode, there are 3 main areas (highlighted in blue in the screenshot) that you'll use to modify your storyboard, from left to right:

1. A scene graph which shows the hierarchy of components in your views.
1. A canvas which shows the distinct views that make up your app - in this case only a single blank screen since you selected the Single View template. The canvas is where you build your interface by laying out UI widgets.
1. A utility area with multiple tabs that provides information about the state of our user interface and a set of tools for modifying it. Among other things, this is where you will find UI widgets to drag into the canvas and where you will size and position elements by value.

---

## Adding a Label in Interface Builder

Now we're ready to start building user interfaces in Xcode.

At the top of the utility area (on the far right side of Xcode), make sure that the **File Inspector** tab is selected:

![alt text][file-inspector]

At the bottom of the utility area, click on the **Object Library** tab:

![alt text][object-library]

The Object Library is where you'll find the user interface elements - the visual building blocks - that Apple provides for creating apps. These elements are part of the [UIKit framework](https://developer.apple.com/documentation/uikit).

![alt text][file-inspector-and-object-library]

In the filter box at the bottom of the **Object Library**, enter the text: **label**. The library will be filtered to show a single item:

![alt text][xcode-label]

Click and drag the label onto the canvas. As you move it closer to the center of the view rectangle, blue lines will appear indicating when it is centered vertically and horizontally. Release the label when it is centered.

Double click the label, type **Hello Mobile Lab!**, and press enter. Recenter the label by clicking and dragging with your mouse.

![alt text][drag-label]

![alt text][centered-label]

---

## Styling and positioning a UILabel

Our label is looking a little generic. Let's kick it up a notch.

With the label still selected, click the **Attributes Inspector** tab at the top right of Interface Builder (second tab from the right).

![alt text][attributes-inspector]

Click on the **Font** field and select the **Custom** option from the font dropdown.

Under the **Family** field, select **Papyrus** and under the **Size** field, select **32.**

Under the **Alignment** field, select the icon that corresponds to center alignment.

![alt text][text-alignment]

As you will notice, our text no longer fits on the screen. There are multiple ways that we can fix this. The solution we'll use is to make the label as big as the view (screen). To start, hold down **ctrl** on your keyboard and drag from the label to the scene graph / hierarchy in the left column. Drag to the option labeled **Safe Area**. When you release your mouse, a popup menu will appear. Select the option - **Equal Widths**.

![alt text][safe-area-drag]

![alt text][set-equal-widths-constraint]

You have just created what's referred to as an **Auto Layout Constraint**. [Auto Layout](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/index.html#//apple_ref/doc/uid/TP40010853-CH7-SW1) is a system for automatically laying out your user interface based on a set of defined constraints.

What's nice about **Auto Layout** is that it relieves us from having to hard code positioning and sizing information. This makes our UI very flexible - for instance our app will automatically respond to different device resolutions without us having to define positioning information for all possible screen sizes. In addition, based on our constraints, **Auto Layout** will respond when a user rotates his/her device between portrait and landscape.

The contraint we just created tells **Auto Layout** that we want our label to have the same width as the "Safe Area" of our view.

Safe Area is a new concept introduced in iOS11. As described in the [documentation](https://developer.apple.com/documentation/uikit/uiview/positioning_content_relative_to_the_safe_area),

> Safe areas help you place your views within the visible portion of the overall interface. UIKit-defined view controllers may position special views on top of your content. For example, a navigation controller displays a navigation bar on top of the underlying view controller’s content. Even when such views are partially transparent, they still occlude the content that is underneath them.

In other words, the Safe Area is a more or less the guaranteed area of the screen where your content will not be obscured by other elements - for example, your content will not be obscured by the infamous [notch](https://www.macrumors.com/2017/11/14/iphone-x-embracing-the-notch) on the iPhoneX that protrudes from the top of the screen.</p>

Repeat the previous step, this time selecting the option - **Equal Heights**. This constraint tells **Auto Layout** that we want our label to have the same height as the **Safe Area** of the view that contains it.

The next two constraints are created in a slightly different way. This time, **Ctrl** drag from the label directly to the canvas (view) that it is placed on. Select the option - **Center Horizontally in Safe Area**. Repeat and select the option - **Center Vertically in Safe Area**.

These last two constraints tell **Auto Layout** to position our label centered both vertically and horizontally within the "Safe Area" of our App.

To summarize, we have created four **Auto Layout Constraints** for our label:

* Equal width to the Safe Area
* Equal height to the Safe Area
* Center horizontally in the Safe Area
* Center vertically in the Safe Area

Your label should now be centered in the Safe Area of the containing view:

![alt text][centered-label]

---

## Running an app on the device simulator

To get a better sense of how our app will look and behave on actual hardware, we can run our code via Xcode’s [Simulator](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/iOS_Simulator_Guide/Introduction/Introduction.html).

At the top left corner of Xcode, you’ll find a drop down menu for selecting [how and where you'd like to build your app](https://developer.apple.com/library/content/featuredarticles/XcodeConcepts/Concept-Targets.html#//apple_ref/doc/uid/TP40009328-CH4-SW1).

![alt text][build-target]

Make sure you do not have any physical iOS devices (iPhone, iPad) plugged into your computer. Click on the dropdown and you’ll see a list of possible **Simulator** targets:

![alt text][build-targets-dropdown]

Select **iPhone X** from the list of targets and then press the adjacent button with the play icon. This builds and runs your app on a simulated iPhone X.

![alt text][build-and-run-button]

It may take some time for the app to build and for the Simulator to run. You can see the current status of this process at the top of the Xcode toolbar. When the toolbar reads **"Running HelloMobileLab on iPhoneX"**, Simulator will have launched your app on a virtual iPhone X. If Simulator did not pop up as the topmost window on your computer, look for it in your dock.

![alt text][running-app-in-simulator]

With Simulator as the topmost window, click on **Hardware** in the toolbar. Play around with some of the menu options. Rotate the virtual device to see how **Auto Layout** responds based on the constraints we setup earlier.

![alt text][simulator-hardware-settings]

<!--
 Extra credit

 Apple's official Getting Started Guide
 https://developer.apple.com/library/content/referencelibrary/GettingStarted/DevelopiOSAppsSwift/BuildABasicUI.html
-->

<!--
  Image references
-->

[create-project-from-welcome]: https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode1.jpg "Create a new Xcode project from the Welcome screen"

[create-project-from-menu]: https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode2.jpg "Create a new Xcode project from the menu"

[single-view-app-template]: https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode3.jpg "Create a new single view app"

[setting-up-a-new-project]: https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode4.jpg "New Xcode project settings"

[interface-builder-anatomy]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode5.jpg "Interface builder anatomy"

[file-inspector]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/xcode-file-inspector.jpg "File Inspector"

[object-library]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/object-library.jpg "Object Library"

[file-inspector-and-object-library]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode6.jpg "File Inspector & Object Library"

[xcode-label]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/label.jpg "Xcode label"

[drag-label]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode7.jpg "Drag label"

[centered-label]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode8.jpg "Centered label"

[attributes-inspector]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode9.jpg "Attributes inspector"

[text-alignment]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/alignment.jpg "Text alignment"

[safe-area-drag]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode12.jpg "Dragging label to safe area"

[set-equal-widths-constraint]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode13.jpg "Setting equal widths constraint"

[centered-label]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode14.jpg "Centered label"

[build-target]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/buildtarget.jpg "Build target"

[build-targets-dropdown]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode15.jpg "Build targets dropdown"

[build-and-run-button]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/build.jpg "Build and run button"

[running-app-in-simulator]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode16.jpg "Running app in simulator"

[simulator-hardware-settings]:https://mobilelaboratory.s3.amazonaws.com/labs/1-xcode/Xcode17.jpg "Simulator hardware settings"