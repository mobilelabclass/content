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