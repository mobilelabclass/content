# Week 5

## Topics 📋

* Data Persistence & Networking
* Developer Tools

## Labs 🔬

* [CocoaPods Tutorial](https://www.appcoda.com/cocoapods/)

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

* After setting up your game controller, you will also need to make a network request to set your profile image and send that info up to the game server. We will be using the [Dog API](https://dog.ceo/dog-api/) to get a random dog image for your profile image.

* Use [Alamofire](https://github.com/Alamofire/Alamofire) to make your network request
  ```swift
    // Make a GET request to the dog api and get a JSON response.
    Alamofire.request("https://dog.ceo/api/breeds/image/random").responseJSON { (responseData) in
        // Print full JSON response.
        print(responseData)
    }
  ```

* Use [SwiftyJSON](https://github.com/SwiftyJSON/SwiftyJSON) to parse your JSON response.
  ```swift
    // Parse JSON response with SwiftyJSON and get message value.
    let json = JSON(responseData.result.value!)
    let dogImageURLString = json["message"].string!
  ```

* Use [SDWebImage](https://github.com/rs/SDWebImage) to download and set your image profile view.
  ```swift
    // Set profile image with the url string.
    self.profileImageView.sd_setImage(with: URL(string: dogImageURLString))
  ```

* Putting it all together, the code block will look something like this:
  ```swift
    // Make a GET request to the dog api and get a JSON response.
    Alamofire.request("https://dog.ceo/api/breeds/image/random").responseJSON { (responseData) in
        // Print full JSON response.
        print(responseData)

        // Parse JSON response with SwiftyJSON and get message value.
        let json = JSON(responseData.result.value!)
        let dogImageURLString = json["message"].string!

        print("DOG IMAGE URL: \(dogImageURLString)")

        // Set profile image with the url string.
        self.profileImageView.sd_setImage(with: URL(string: dogImageURLString))
    }
  ```

* After setting the profile image, send the image url string to the game server.
  ```swift
    // Send the playerId and dog image url string in this format.
    let message = "\(playerId), profile_image:\(dogImageURLString)"
    socket?.write(string: message) {
        print("⬆️  profile image sent message to server: ", message)
    }
  ```

* A random dog image should be set and sent to the game server only when a *new player id is saved.*

* Save the image url in the UserDefaults so it gets reloaded when the app restarts.

* **Deploy your app via TestFlight before the start of class**.

* The following are important:
  * Review the CocoaPods tutorial in the Labs section. The example is for Firebase, but the steps are the same for any library.
  * Run **pod install** from the command line within the project directory
  * Be sure to run **pod install** anytime an update is made to the Podfile.   
    e.g. After adding the Alamofire, SwiftyJSON, and SDWebImage pods.
  * See the Github page for the libraries to get the correct pod version.   
    e.g. **pod 'Alamofire', '~> 4.6'**
  * **Open the code kit from .xcworkspace and NOT .xcodeproj**
  * Remember to include the *import* statements in your swift code to use the library routines.

[controller-directions]:https://mobilelaboratory.s3.amazonaws.com/week5/directions.png "controller-directions"
