# Using WebDriver

## From a New Download

There are two [downloads](http://code.google.com/p/selenium/downloads/list) that you will generally want to chose:

  * `selenium-server-standalone.jar` - This contains the Selenium server (required if you want to use the Selenium 1.x classes) and the RemoteWebDriver server as a standalone jar, that can be executed using `java -jar`. Note that this JAR contains _all_ of the required dependencies. If you'd prefer to manage the classpath yourself, then download the "selenium-server-XXX.zip" file.

  * The language bindings for your language of choice. If you're using Java, then download the latest `selenium-java-XXX.zip` where "XXX" represents the version number. For .Net, the appropriate zip is the `selenium-dotnet-XXX.zip`.

## With Maven

To use the client libraries (the remote webdriver client, ChromeDriver, FirefoxDriver, HtmlUnitDriver, InternetExplorerDriver, OperaDriver, and support libraries) Add the following dependencies to your project:

```
  <dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>2.26.0</version>
  </dependency>
```

If you want to start a selenium server, you will need the following dependency:

```
  <dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-server</artifactId>
    <version>2.26.0</version>
  </dependency>
```

That's it.

## From a Fresh CheckOut

  * Follow the instructions in CheckOut in order to get the latest version of WebDriver.
  * Run `rake` without any targets (that is, just run `rake`) This will run the unit tests too, and if that's not what you want to do, just run: `rake all`
  * Figure out which drivers you will need on your project. You'll need at least **common**, but depending on your needs you may or may not want the drivers for **Firefox** (FirefoxDriver), **Internet Explorer** (InternetExplorerDriver), and/or **HtmlUnit** (HtmlUnitDriver) The JARs for the classes in "org.openqa.selenium" are located in subdirectories of the "build" directory. It should be obvious what is where. The OperaDriver is located in _/third\_party/java/opera-driver/_.
  * Now, just copy the required runtime libraries and the newly built jars on to your CLASSPATH. Read the `build.desc` files to figure out what these are.