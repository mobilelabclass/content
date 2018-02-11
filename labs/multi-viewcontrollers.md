# Multi ViewControllers

[![multi](https://user-images.githubusercontent.com/206423/36069382-7bc69448-0eb6-11e8-950c-2af7cb843cf2.png)](https://youtu.be/Utb8rz5igkY)

Simple app setup with two UIViewControllers.

```swift
// Present second view controller.
let storyboard = UIStoryboard(name: "Main", bundle: nil)

let secondViewController = storyboard.instantiateViewController(withIdentifier: "SecondViewController")

self.present(secondViewController, animated: true, completion: nil)
```

```swift
// Dismiss second view controller.
self.dismiss(animated: true, completion: nil)
```

