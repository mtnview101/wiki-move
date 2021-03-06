# The 5 Minute Getting Started Guide

WebDriver is a tool for automating testing web applications, and in particular to verify that they work as expected. It aims to provide a friendly API that's easy to explore and understand, which will help make your tests easier to read and maintain. It's not tied to any particular test framework, so it can be used equally well with JUnit, TestNG or from a plain old "main" method. This "Getting Started" guide introduces you to WebDriver's Java API and helps get you started becoming familiar with it.

Start by [downloading](http://code.google.com/p/selenium/downloads/list) the latest binaries and unpack them into a directory. A safe choice is the latest version of `selenium-server-standalone-`_x.y.z_`.jar`  _x_, _y_ and _z_ will be digits e.g. 2.4.0 at the time of writing. From now on, we'll refer to this directory as `$WEBDRIVER_HOME`. Now, open your favourite IDE and:

  * Start a new Java project in your favourite IDE
  * Add all the JAR files under `$WEBDRIVER_HOME` to the `CLASSPATH`

You can see that WebDriver acts just as a normal Java library does: it's entirely self-contained, and you don't need to remember to start any additional processes or run any installers before using it.

You're now ready to write some code. An easy way to get started is this example, which searches for the term "Cheese" on Google and then outputs the result page's title to the console. You'll start by using the HtmlUnitDriver. This is a pure Java driver that runs entirely in-memory. Because of this, you won't see a new browser window open.

```
package org.openqa.selenium.example;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.htmlunit.HtmlUnitDriver;

public class Example  {
    public static void main(String[] args) {
        // Create a new instance of the html unit driver
        // Notice that the remainder of the code relies on the interface, 
        // not the implementation.
        WebDriver driver = new HtmlUnitDriver();

        // And now use this to visit Google
        driver.get("http://www.google.com");

        // Find the text input element by its name
        WebElement element = driver.findElement(By.name("q"));

        // Enter something to search for
        element.sendKeys("Cheese!");

        // Now submit the form. WebDriver will find the form for us from the element
        element.submit();

        // Check the title of the page
        System.out.println("Page title is: " + driver.getTitle());

        driver.quit();
    }
}

```

Compile and run it. You should see a line with the title of the Google search results as output. Congratulations, you've managed to get started with WebDriver!

In this next example, let's use a page that requires Javascript to work properly, such as Google Suggest. You will also be using the FirefoxDriver. Make sure that Firefox is installed on your machine and is in the normal location for your OS.

Once that's done, create a new class called GoogleSuggest, which looks like:

```
package org.openqa.selenium.example;

import java.util.List;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.firefox.FirefoxDriver;

public class GoogleSuggest {
    public static void main(String[] args) throws Exception {
        // The Firefox driver supports javascript 
        WebDriver driver = new FirefoxDriver();
        
        // Go to the Google Suggest home page
        driver.get("http://www.google.com/webhp?complete=1&hl=en");
        
        // Enter the query string "Cheese"
        WebElement query = driver.findElement(By.name("q"));
        query.sendKeys("Cheese");

        // Sleep until the div we want is visible or 5 seconds is over
        long end = System.currentTimeMillis() + 5000;
        while (System.currentTimeMillis() < end) {
            WebElement resultsDiv = driver.findElement(By.className("gssb_e"));

            // If results have been returned, the results are displayed in a drop down.
            if (resultsDiv.isDisplayed()) {
              break;
            }
        }

        // And now list the suggestions
        List<WebElement> allSuggestions = driver.findElements(By.xpath("//td[@class='gssb_a gbqfsf']"));
        
        for (WebElement suggestion : allSuggestions) {
            System.out.println(suggestion.getText());
        }

        driver.quit();
    }
}
```

When you run this program, you'll see the list of suggestions being printed to the console. That's all there is to using WebDriver!

Hopefully, this will have whet your appetite for more. In the NextSteps page, you will learn how more about how to use WebDriver. It covers aspects such as navigating forward and backward in your browser's history, how to use frames and windows and it provides a more complete discussion of the examples than has been done as you've been GettingStarted. If you're ready, let's take the NextSteps!