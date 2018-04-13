# Raspberry Pi

In this lab we'll break out the breadboard and explore how to communicate with the [Raspberry Pi](https://www.raspberrypi.org/) via HTTP and Bluetooth. We'll write a simple iOS app that controls a servo motor attached to a Pi.

For this lab you'll need a servo motor, a breadboard, hookup wire, and a Raspberry Pi 3.

## Controlling a servo on Raspberry Pi

Complete the [Getting Started](https://projects.raspberrypi.org/en/projects/raspberry-pi-getting-started) instructions on the official Raspberry Pi website to install the [Raspbian](https://www.raspbian.org/) operating system and connect to your Pi.

The hardware setup for this lab is quite simple - connecting a servo to the Raspberry Pi is exactly the same as with the Arduino (see the ITP Physical Computing instructions for Arduino [here](https://itp.nyu.edu/physcomp/labs/labs-arduino-digital-and-analog/servo-motor-control-with-an-arduino/).

In our setup, we power our Raspberry Pi from the +5.1V micro USB supply.

All we need to do is connect 3 wires from the servo to 3 GPIO pins on the Pi. The red wire goes to +5V, the black wire to ground, and the yellow (control) wire to pin #18. Adafruit has a detailed write-up on how to setup this simple circuit [here](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-8-using-a-servo-motor/hardware)

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
}
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


Defining Services and Characteristics in bleno is straightforward. Add the following to index.js to define our servo service and servo position characteristic:

```javascript
  var PrimaryService = bleno.PrimaryService;
  var Characteristic = bleno.Characteristic;
  var Descriptor = bleno.Descriptor;

  var myServoService = new PrimaryService({
    uuid: '8b6a02cc-e6cf-4af9-9138-3356913b8a14',
    characteristics: [
      new Characteristic({
          uuid: '316cd3b8-4303-4d30-81c4-59c2774668e5',
          properties: ['read', 'write'],
          descriptors: [
              new bleno.Descriptor({
                  uuid: '169c58d3-3058-45bc-b8c2-e132c864b8d5',
                  value: 'servo position'
              })
          ]
      })
    ]
  });
```

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

## Bluetooth on iOS with CoreBluetooth

Apple Docs:
https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/AboutCoreBluetooth/Introduction.html

https://developer.apple.com/documentation/corebluetooth


## Further reading
* https://medium.com/@superlopuh/raspberry-pi-ios-communication-in-bluetooth-c7599e257f2


<!--
  Image references
-->

[ble-diagram]: https://cdn-learn.adafruit.com/assets/assets/000/013/828/large1024/microcontrollers_GattStructure.png?1390836057 "BLE Diagram"

