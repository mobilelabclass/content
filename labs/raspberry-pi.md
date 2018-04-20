# Raspberry Pi

In this lab we'll break out the breadboard and explore how to communicate with the [Raspberry Pi](https://www.raspberrypi.org/) via HTTP and Bluetooth. We'll write a simple iOS app that controls a servo motor attached to a Pi.

For this lab you'll need a servo motor, a breadboard, hookup wire, and a Raspberry Pi 3.

## Controlling a servo on Raspberry Pi

Complete the [Getting Started](https://projects.raspberrypi.org/en/projects/raspberry-pi-getting-started) instructions on the official Raspberry Pi website to install the [Raspbian](https://www.raspbian.org/) operating system and connect to your Pi.

The hardware setup for this lab is quite simple - connecting a servo to the Raspberry Pi is exactly the same as with the Arduino (see the ITP Physical Computing instructions for Arduino [here](https://itp.nyu.edu/physcomp/labs/labs-arduino-digital-and-analog/servo-motor-control-with-an-arduino/).

In our setup, we power our Raspberry Pi from the +5.1V micro USB supply.

All we need to do is connect 3 wires from the servo to 3 GPIO pins on the Pi. The red wire goes to +5V, the black wire to ground, and the yellow (control) wire to pin #18. Your circuit should look like the following: 

![alt-text][raspberry-pi-servo-circuit]


Adafruit has a detailed write-up on how to setup this simple circuit [here](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-8-using-a-servo-motor/hardware)

To read more about the various pins on the Pi, visit the [Pinout website](https://pinout.xyz).


## Raspberry Pi Software Setup

We'll use the [WiringPi](http://wiringpi.com/) GPIO library to control our servo. More specifically, we'll use the [wiringpi-node](https://www.npmjs.com/package/wiringpi-node) npm package so we can use WiringPi in Javascript.

Connect to your Pi and open the Terminal.

Create a new folder for your project:

```
mkdir hellopi
cd hellopi
```

Create a package.json file to manage our project dependencies:

```
npm init
```

Breeze through the questions at the interactive prompt by just pressing enter:
* **package name >** ENTER
* **version >** ENTER
* **description >** ENTER
* . . .
* **Is this ok? >** ENTER


Open package.json in your favorite text editor on Pi (e.g. Leafpad).

Add the following entry to the json file:
```json
"dependencies": {
    "wiringpi-node": "2.4.4"
  }
```

Your package.json file should now look something like this:
```json
{
  "name": "hellopi",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "wiringpi-node": "2.4.4"
  },
  "author": "",
  "license": "ISC"
}
```

Back in Terminal, install the dependency we just added:

```
npm install
```

When installation completes, create the entry point to our app:

```
touch index.js
```

Open index.js in a text editor.

The following snippet imports the wiringpi-node library, configures the board for communication via PWM, and sends simple servo move commands in a setInterval loop:

```javascript
// Import wiringpi-node
var wpi = require('wiringpi-node');

// Setup GPIO
wpi.wiringPiSetupGpio();

// Configure the board for PWM
wpi.pinMode(18, wpi.PWM_OUTPUT)
wpi.pwmSetMode(wpi.PWM_MODE_MS)
wpi.pwmSetClock(192)
wpi.pwmSetRange(2000)

// Control variables
var delay = 20; // milliseconds
var direction = 1;
var pulse = 100;
var pin = 18;
var minPulse = 100;
var maxPulse = 200;

// Our "run loop"
setInterval(function() {
  // Increment the pulse value in the direciton we are moving
  pulse += direction;

  // Write to the pin
  wpi.pwmWrite(pin, pulse);

  // Reverse direction if outside min/max pulse range
  if (pulse >= maxPulse) {
    direction = -1;
  }

  if (pulse <= 100) {
    direction = 1;
  }

}, delay);
```

Back in terminal, running our script makes our servo move back and forth continously:
```
  sudo node index.js
```

## Bluetooth Setup on Raspberry Pi

We'll use the [Bleno](https://github.com/noble/bleno) javascript library to turn our Pi into a bluetooth peripheral. Bleno uses the [BlueZ](http://www.bluez.org/) Bluetooth protocol stack. As of the writing of this lab (April 2018), BlueZ comes preinstalled on Raspberry Pi.

To get started, we'll first need to stop the bluetooth daemon. Make sure that the keyboard and mouse you are using are wired before running the following command:

```
sudo systemctl stop bluetooth
```

You can verify that the daemon is stopped by running:

```
sudo systemctl status bluetooth
```

The command we ran previously only stops bluetooth temporarily. When you restart your Pi, bluetooth connectivity will be restored. To stop the daemon permanently,run the following:

```
sudo systemctl disable bluetooth
```

Manually power on the bluetooth adapter:
```
sudo hciconfig hci0 up
```

Finally, install libudev-dev, a library for developing applications that use udev (a device manager for the linux kernal):

```
sudo apt-get update
sudo apt-get install libudev-dev
```

## Communicating via Bluetooth

Update package.json, adding bleno as a dependency:

```json
{
  "name": "hellopi",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "bleno": "0.5.0",
    "wiringpi-node": "2.4.4"
  },
  "author": "",
  "license": "ISC"
}
```

Install the new dependency:

```
npm install
```

Once Bleno has been installed, we need to import it at the top of our index.js file. Add the following line to the top of index.js:

```javascript
  var bleno = require('bleno');
```

Next, we can add an event handler for Bluetooth state changes;

```javascript
bleno.on('stateChange', function(newState) {
  console.log('bleno state changed:', newState);
});
```

Running our app again, we should see the following message in terminal:

```
  bleno state changed: poweredOn
```

It's alive! Hooray! Now we can start advertising our Raspberry Pi as a Bluetooth device.

Add the following to the top of index.js:
```javascript
var name = 'Awesome Bluetooth Servo';
var uuid = '5f694e5e-9af7-4ecd-a12b-80e1e3b2e480';
```

5f694e5e-9af7-4ecd-a12b-80e1e3b2e480 is a UUID we generated [here](https://www.uuidgenerator.net/). Make sure to generate your own UUID to avoid conflicts.

 Add the following within the callback function for state changed:

```javascript
if (state === 'poweredOn') {
  bleno.startAdvertising(name, [uuid]);
} else {
  bleno.stopAdvertising();
```

In its entirety, the block should look like this:
```javascript
bleno.on('stateChange', function(newState) {
  console.log('bleno state changed:', newState);
  
  if (newState === 'poweredOn') {
    bleno.startAdvertising(name, serviceUuids);
  } else {
    bleno.stopAdvertising();
  }
});
```

## Bluetooth Advertising, Services and Characteristics

To communicate with the outside world, our Raspberry Pi needs to meet two specifications - [General Access Profile (GAP)](https://learn.adafruit.com/introduction-to-bluetooth-low-energy/gap) and [Generic Attribute Profile (GATT)](https://learn.adafruit.com/introduction-to-bluetooth-low-energy/gatt).

Together these specifications define how devices makes themselves discoverable and how two BLE devices transfer data back and forth.

We recommend reading Adafruit's [Intro to Bluetooth](https://learn.adafruit.com/introduction-to-bluetooth-low-energy) for more background on how GAP and GATT work.

TL;DR

* BLE devices advertise themselves according to their role - **peripheral device** (e.g. Raspberry Pi) or **central device** (e.g. iPhone). In the previous section we started advertising our Raspberry Pi with the bleno library.

* Devices communicate according to a client/server relationship, where the central device is the client and the peripheral is the server. The client initiates communication (called a GATT transaction) to which the server may respond.


* Data is exchanged based on the concept of **Profiles**, **Services**, and **Characteristics**.
  
  * **Profile** - a collection of Services

  * **Service** - Identitified by a unique UUID, services break up data into logical entities. The full list of officially adopted BLE Services is available [here](https://www.bluetooth.com/specifications/gatt/services). Services can have one or more pieces of data - each of which makes up a Characteristic.

  * **Characteristic** - Encapsulates a single data point, for example a sensor reading.

Diagram from Adafruit's [Intro to Bluetooth](https://learn.adafruit.com/introduction-to-bluetooth-low-energy).

![alt-text][ble-diagram]


Defining Services and Characteristics in bleno is straightforward. Add the following to index.js to define our **servo service** and **servo position characteristic**:

```javascript
  var PrimaryService = bleno.PrimaryService;
  var Characteristic = bleno.Characteristic;

  var serviceUuid = 'e853db91-e787-4eeb-ae7c-536d689f5741';
  var characteristicUuid = '01ad6336-32b5-499c-9130-3f989684044c';

  var myServoService = new PrimaryService({
    uuid: serviceUuid,
    characteristics: [
      new Characteristic({
          uuid: characteristicUuid,
          properties: ['read', 'write'],
      })
    ]
  });
```

Also add the following lines to debug issues with advertising our device:

```javascript
bleno.on('advertisingStart', function(err) {
  if (!err) {
      console.log('Started advertising device with uuid', uuid);

  } else {
    console.log('Error advertising our BLE device!', err);
  }
});
```
## Debugging Bluetooth
The LightBlue Explorer app for iOS is a great way to debug BLE devices. You can download it on [the App Store](https://itunes.apple.com/us/app/lightblue-explorer/id557428110?mt=8). The app allows you to scan for and connect to all BLE devices around you. Once connected, you have a detailed view of all the device's profiles, and can even read / write values to characteristics.

Run the javascript file on our Raspberry Pi:

```shell
  sudo node index.js
```

In the LightBlue Explorer app, you should be able to see your device:

![alt-text][bluetooth-app]

Tapping on "My Awesome Servo" connects to the peripheral and displays additional information:

![alt-text][bluetooth-app2]


## Bluetooth on iOS with CoreBluetooth

Bluetooth on iOS is implemented via the [CoreBluetooth](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/AboutCoreBluetooth/Introduction.html) framework. Code for the iOS portion of this lab is available on [github](https://github.com/mobilelabclass/mobile-lab-raspberry-pi-kit).

In our main View Controller, we create an instance of  [CBCentralManager](https://developer.apple.com/documentation/corebluetooth/cbcentralmanager) and assign the view controller as the manager's delegate.

```swift
  centralManager = CBCentralManager(delegate: self, queue: nil)
```

We also declare that our View Controller implements the [CBCentralManagerDelegate](https://developer.apple.com/documentation/corebluetooth/cbcentralmanagerdelegate) protocol at the top of our file:

```swift
  class ViewController: UIViewController, CBCentralManagerDelegate {
  }
```

As stated in the official docs, the optional methods of the CBCentralManagerDelegate protocol allow the delegate to monitor the discovery, connectivity, and retrieval of peripheral devices. The only required method of the protocol indicates the availability of the central manager, and is called when the central manager’s state is updated.

We implement the required protocol method in our view controller:

```swift
func centralManagerDidUpdateState(_ central: CBCentralManager) {
  if (central.state == .poweredOn) {
    // Scan for all peripherals
    central.scanForPeripherals(withServices: nil, options: [CBCentralManagerScanOptionAllowDuplicatesKey: false])
  }
}
```

This method is called whenever the state of our central manager changes. In the method we check whether the new state is "powered on" and call a method on the central manager to scan for bluetooth peripherals if the condition is true.

To be informed when peripherals are discovered, we implement another method of the CentralManagerDelegate protocol:

```swift
func centralManager(_ central: CBCentralManager, didDiscover peripheral: CBPeripheral, advertisementData: [String : Any], rssi RSSI: NSNumber) {
  if (peripheral.name == PERIPHERAL_NAME) {
    // Assign peripheral to myPi
    myPi = peripheral
          
    // Connect to the peripheral
    centralManager?.connect(peripheral, options: nil)
  }
}
```

In the above block of code, we check whether the discovered peripheral's name matches the name we assigned our Raspberry Pi - "My Awesome Servo". If it does, we assign the peripheral to a variable and call another method on the central manager which attempts to connect to it.

You may have started to notice that the process for discovering and interacting with bluetooth peripherals follows an asynchronous pattern - we make a request and at some point in the future the request returns. When it returns, the response may contain some new information that we can act upon.

Our flow so far looks like the following:

![alt-text][ios-bluetooth-flow1]

In our view controller we make requests like "scan for peripherals" and at some time in the future, our associated delegate method gets called - "did discover peripheral". By following this pattern down the hierarchy of Peripheral- Services - Characteristics, we eventually work our way to the individual characteristics we want to interface with. In our case, the servo position characterstic we created earlier.

The complete flow for getting a handle on the servo position characteristic looks like the following:

![alt-text][ios-bluetooth-flow2]

Once we have a reference to a characteristic, we can save it to a variable for use in our application. In the example app we have a slider that we use to control servo position. In a






## Further reading
* https://medium.com/@superlopuh/raspberry-pi-ios-communication-in-bluetooth-c7599e257f2


<!--
  Image references
-->

[ble-diagram]: https://cdn-learn.adafruit.com/assets/assets/000/013/828/large1024/microcontrollers_GattStructure.png?1390836057 "BLE Diagram"

[bluetooth-app]: http://mobilelaboratory.s3.amazonaws.com/IoT/lightblue-1.png "Bluetooth app screenshot"

[bluetooth-app2]: http://mobilelaboratory.s3.amazonaws.com/IoT/lightblue-3.png "Bluetooth app screenshot 2"

[ios-bluetooth-flow1]:http://mobilelaboratory.s3.amazonaws.com/IoT/flow1.png
"iOS bluetooth flow 1"

[ios-bluetooth-flow2]:http://mobilelaboratory.s3.amazonaws.com/IoT/flow2.png
"iOS-bluetooth-flow-2"

[raspberry-pi-servo-circuit]:http://mobilelaboratory.s3.amazonaws.com/IoT/pi-diagram.png "Raspberry pi servo circuit"