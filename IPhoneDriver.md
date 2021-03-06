# automating iOS

If you are looking to use WebDriver with iOS mobile Safari and are currently testing only on simulators please have a look at [ios-driver](http://ios-driver.github.io/ios-driver/) or [appium](http://appium.io/)

Both of these projects are much better implementations of WebDriver for iOS, the only reason it is not a wholesale replacement now is that they can not run on real devices for mobile safari right now.

# iPhone Driver (DEPRECATED)

The iphone driver allows testing on a UIWebView (a webkit browser accessible for 3rd party applications) on the iphone. It works through the use of an iphone application running on your iphone, ipod touch or iphone simulator.

## Drawbacks

  * To test on a device you need the iphone SDK and a provisioning profile (for running on an actual device)

# Building
From your [checked-out copy of the repo](http://code.google.com/p/selenium/source/checkout), and checkout the last git tag that the source was available:
```
git checkout selenium-2.38.0
```
then run:
```
./go iphone
```
Which will build the iPhone driver and the application to be installed. It's easier to start up the SDK manually and run it.

  * if you are on Lion and have installed Xcode through the App Store and you get this message "XCode not found. Not building the iphone driver."  try running this on the command line: "sudo /usr/bin/xcode-select -switch /Applications/Xcode.app"

# Installing

The iphone driver application is not currently on the Apple store. To run it, you will need the iphone development tools on your local machine. These tools must be downloaded from [apple](http://developer.apple.com/xcode/index.php) at least version 4.2 at the time of this writing. To run it on your device, you will also need a provisioning profile (which requires a developer license).

The iphone driver connects through HTTP to the iphone, ipod or iphone simulator. You can run the simulator on another machine on your network and configure webdriver to connect to it remotely.

## In the simulator

First, install the Xcode (at least version 4.6) from here: https://developer.apple.com/xcode/index.php

Download the source from [here](http://code.google.com/p/selenium/source/checkout) and open selenium/iphone/iWebDriver.xcodeproj in xcode.

Set your build configuration to Simulator / iPhone OS 5.0 / iWebDriver. This is done in a drop-down box in the top left of the project window.

Click Build & Go. After compiling, the iphone simulator should appear and the iWebDriver app will be installed in it.

## On the device

For now, you will need the full set of development tools from apple to install on your device.

Install the iphone SDK and configure your build environment as described above. You will also need a provisioning profile from Apple to be installed and configured for your device.

Download and open the iWebdriver project as above. Open `Info.plist` and edit the Bundle Identifier to `com.NAME.${PRODUCT_NAME:identifier}` where NAME is the name you registered your provisioning profile to be an authority on.
  * TIP: You don't seem to need to edit the NAME If you use the default provisioning profile which may simply use a wildcard for all domains)

Make sure your device is connected to your computer. Your device must also be routable from your computer. The easiest way to do this is to configure a wifi network and connect your device to it.

Set your build configuration to Device / iPhone OS 5.0 / iWebDriver and click Build & Go.

### Tips and Traps
To get the project to build for a device in xcode 4.3.3 several additional steps were needed in the project's `Build Settings` tab.

For the `Architectures` section:
  * `Architectures` is set to `Standard armv7`
  * Base SDK is set to `Latest iOS (iOS 5.1)` - note: the value in brackets may change as new versions of iOS become generally available
  * `Valid Architectures` add `armv7` I also have `i386` as a separate entry.

For the `Code Signing` section I picked `iPhone Developer` for all the entries in the `Code Signing Identity` section (the first available entry in the recommended `Automatic Profile Selector` part of the drop-down list.

## Connecting

To start using the iphone driver, make sure the iWebDriver project has started (blank screen with url at the bottom) and then connect to it with RemoteWebDriver.

Java -
```
WebDriver driver = new RemoteWebDriver(new URL("http://localhost:3001/wd/hub"), DesiredCapabilities.iphone());
// or use the convenience class which uses localhost:3001 by default
WebDriver driver = new IPhoneDriver();
```

C# -
```
IWebDriver driver = new RemoteWebDriver(new Uri("http://localhost:3001/wd/hub"), DesiredCapabilities.IPhone());
```

Ruby -
```
driver = Selenium::WebDriver.for :remote, :url => "http://localhost:3001/wd/hub", :desired_capabilities => :iphone
# or use the convenience class which uses localhost:3001 by default
driver = Selenium::WebDriver.for :iphone
```

Python -
```
driver = webdriver.Remote("http://localhost:3001/wd/hub", webdriver.DesiredCapabilities.IPHONE)
```

### Connection tips

To connect with an actual device replace `localhost` with the ip address that's displayed on the screen of the  iWebDriver server app running on your device (once you've started it). e.g. `http://192.168.1.23:3001/wd/hub` if your device has the ip address `192.168.1.23`

If your development machine where you're running the WebDriver code cannot 'see' the iPhone or iPad then try 'pinging' the ip address displayed in the WebDriver server app once it's started on the device. There are various network utilities available to run on iPads and iPhones that can also 'ping' your development machine e.g. [Typhuun System Scope Lite](http://www.typhuun.com/systemscope.html) which managed to convince my development machine my iPad was reachable.

## Hooking it up to a Selenium Grid2
Once you have the app installed on your device or emulator. Go to the iOS Settings app. Near the bottom there will be an "iWebDriver" settings area. Going into that you should see settings for Server port and Grid Host/Port. Fill in the details as desired.