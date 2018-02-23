# Week 5

## Topics 📋

* Data Persistence & Networking
* Developer Tools

## Labs 🔬

* [CocoaPods Tutorial](https://www.appcoda.com/cocoapods/)
* Networking Requests (will post link)

## Design Assignment 📐

**Multiplayer game controller**

Next week we'll reveal a multiplayer game that you'll all play together as a class. We have already written the game server - you will each create your own game controller to send the server commands over websockets.

* Create your own multiplayer game controller using the [Web Socket Kit](https://github.com/mobilelabclass/mobile-lab-websocket-kit).
* Your controller will be used to move an avatar around a level using a simple communications protocol - the structure of a valid message is as follows:

  ```swift
    [username], [direction]

    // e.g. ⬆️: "redburns, 0"
    // e.g. ➡️: "redburns, 1"
    // e.g. ⬇️: "redburns, 2"
    // e.g. ⬅️: "redburns, 3"
  ```

  Direction is a number between 0 and 3:
  ![alt text][controller-directions]

* You can test whether your app is functioning properly by loading [this test environment](http://websockets.mobilelabclass.com/) in your browser. When you have sent a successful command to the server you will see a circle representing your player move on screen.

* Think creatively about your controller mechanics - use custom UI, gestures, or motion to make your controller more interesting. Is your design intent to give the user a competitive advantage? To challenge the user? To experiment with an absurd mechanic?

* The following are important:
  * Install Cocoapods. Instructions are available [here](www.cocoapods.org)
  * Run **pod install** from the command line within the project directory
  * **Open the code kit from .xcworkspace and NOT .xcodeproj**

[controller-directions]:https://mobilelaboratory.s3.amazonaws.com/week5/directions.png "controller-directions"
