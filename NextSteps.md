# Next Steps For Using WebDriver

Once you've completed the GettingStarted tutorial, you may want to know more. This is the right place to look!

## Which Implementation of WebDriver Should I Use?

WebDriver is the name of the key interface against which tests should be written, but there are several implementations. These are:

| **Name of driver** | **Available on which OS?** | **Class to instantiate** |
|:-------------------|:---------------------------|:-------------------------|
| HtmlUnitDriver     | All                        | org.openqa.selenium.htmlunit.HtmlUnitDriver |
| FirefoxDriver      | All                        | org.openqa.selenium.firefox.FirefoxDriver |
| InternetExplorerDriver | Windows                    | org.openqa.selenium.ie.InternetExplorerDriver |
| ChromeDriver       | All                        | org.openqa.selenium.chrome.ChromeDriver |
| OperaDriver        | All                        | com.opera.core.systems.OperaDriver |

You can find out more information about each of these by following the links in the table. Which you use depends on what you want to do. For sheer speed, the HtmlUnitDriver is great, but it's not graphical, which means that you can't watch what's happening. As a developer, you may be comfortable with this, but sometimes it's good to be able to test using a real browser, especially when you're showing a demo of your application (or running the tests) for an audience. Often, this idea is referred to as "safety", and it falls into two parts. Firstly, there's "actual safety", which refers to whether or not the tests works as they should. This can be measured and quantified. Secondly, there's "perceived safety", which refers to whether or not an observer _believes_ the tests work as they should. This varies from person to person, and will depend on their familiarity with the application under test, WebDriver and your testing framework.

To support higher "perceived safety", you may wish to choose a driver such as the FirefoxDriver. This has the added advantage that this driver actually renders content to a screen, and so can be used to detect information such as the position of an element on a page, or the CSS properties that apply to it. However, this additional flexibility comes at the cost of slower overall speed. By writing your tests against the WebDriver interface, it is possible to pick the most appropriate driver for a given test.

To keep things simple, let's start with the HtmlUnitDriver:

```
WebDriver driver = new HtmlUnitDriver();
```

## Navigating

The first thing you'll want to do with WebDriver is navigate to a page. The normal way to do this is by calling "get":

```
driver.get("http://www.google.com");
```

WebDriver will wait until the page has fully loaded (that is, the "onload" event has fired) before returning control to your test or script.

## Interacting With the Page

Just being able to go to places isn't terribly useful. What we'd really like to do is to interact with the pages, or, more specifically, the HTML elements within a page. First of all, we need to find one. WebDriver offers a number of ways of finding elements. For example, given an element defined as:

```
<input type="text" name="passwd" id="passwd-id" />
```

we could use any of:

```
WebElement element;
element = driver.findElement(By.id("passwd-id"));
element = driver.findElement(By.name("passwd"));
element = driver.findElement(By.xpath("//input[@id='passwd-id']"));
```

You can also look for a link by its text, but be careful! The text must be an _exact_ match! You should also be careful when using XpathInWebDriver. If there's more than one element that matches the query, then only the first will be returned. If nothing can be found, a `NoSuchElementException` will be thrown.

WebDriver has an "Object-based" API; we represent all types of elements using the same interface: WebElement. This means that although you may see a lot of possible methods you could invoke when you hit your IDE's auto-complete key combination, not all of them will make sense or be valid. Don't worry! WebDriver will attempt to do the Right Thing, so if you call a method that makes no sense ("setSelected()" on a "meta" tag, for example) an exception will be thrown.

So, we've got an element. What can we do with it? First of all, you may want to enter some text into a text field:

```
element.sendKeys("some text");
```

You can simulate pressing the arrow keys by using the "Keys" class:

```
element.sendKeys(" and some", Keys.ARROW_DOWN);
```

It is possible to call `sendKeys` on any element, which makes it possible to test keyboard shortcuts such as those used on GMail. A side-effect of this is that typing something into a text field won't automatically clear it. Instead, what you type will be appended to what's already there. You can easily clear the contents of a text field or textarea:

```
element.clear();
```

## Filling In Forms

We've already seen how to enter text into a textarea or text field, but what about the other elements? You can "toggle" the state of checkboxes, and you can use "setSelected" to set something like an OPTION tag selected. Dealing with SELECT tags isn't too bad:

```
WebElement select = driver.findElement(By.xpath("//select"));
List<WebElement> allOptions = select.findElements(By.tagName("option"));
for (WebElement option : allOptions) {
    System.out.println(String.format("Value is: %s", option.getAttribute("value")));
    option.click();
}
```

This will find the first "SELECT" element on the page, and cycle through each of it's OPTIONs in turn, printing out their values, and selecting each in turn. As you can see, this isn't the most efficient way of dealing with SELECT elements. WebDriver's support classes come with one called "`Select`", which provides useful methods for interacting with these.

Once you've finished filling out the form, you probably want to submit it. One way to do this would be to find the "submit" button and click it:

```
driver.findElement(By.id("submit")).click();  // Assume the button has the ID "submit" :)
```

Alternatively, WebDriver has the convenience method "submit" on every element. If you call this on an element within a form, WebDriver will walk up the DOM until it finds the enclosing form and then calls submit on that. If the element isn't in a form, then the "NoSuchElementException" will be thrown:

```
element.submit();
```

## Drag And Drop

You can use drag and drop, either moving an element by a certain amount, or on to another element:

```
WebElement element = driver.findElement(By.name("source"));
WebElement target = driver.findElement(By.name("target"));

(new Actions(driver)).dragAndDrop(element, target).perform();
```

## Moving Between Windows and Frames

It's rare for a modern web application not to have any frames or to be constrained to a single window. WebDriver supports moving between named windows using the "switchTo" method:

```
driver.switchTo().window("windowName");
```

All calls to driver will now be interpreted as being directed to the particular window. But how do you know the window's name? Take a look at the javascript or link that opened it:

```
<a href="somewhere.html" target="windowName">Click here to open a new window</a>
```

Alternatively, you can pass a "window handle" to the "switchTo().window()" method. Knowing this, it's possible to iterate over every open window like so:

```
for (String handle : driver.getWindowHandles()) {
  driver.switchTo().window(handle);
}
```

You can also swing from frame to frame (or into iframes):

```
driver.switchTo().frame("frameName");
```

It's possible to access subframes by chaining switchTo() calls, and you can specify the frame by its index too. That is:

```
driver.switchTo().frame("frameName")
      .switchTo().frame(0)
      .switchTo().frame("child");
```

would go to the frame named "child" of the first subframe of the frame called "frameName". **All frames are evaluated as if from currently switched to frame**.  To get back to the top level, call:

```
driver.switchTo().defaultContent();
```

## Navigation: History and Location

Earlier, we covered navigating to a page using the "get" command (`driver.get("http://www.example.com")`) As you've seen, WebDriver has a number of smaller, task-focused interfaces, and navigation is a useful task. Because loading a page is such a fundamental requirement, the method to do this lives on the main WebDriver interface, but it's simply a synonym to:

```
driver.navigate().to("http://www.example.com");
```

To reiterate: "navigate().to()" and "get()" do exactly the same thing. One's just a lot easier to type than the other!

The "navigate" interface also exposes the ability to move backwards and forwards in your browser's history:

```
driver.navigate().forward();
driver.navigate().back();
```

Please be aware that this functionality depends entirely on the underlying browser. It's just possible that something unexpected may happen when you call these methods if you're used to the behaviour of one browser over another.

## Cookies

Before we leave these next steps, you may be interested in understanding how to use cookies. First of all, you need to be on the domain that the cookie will be valid for:

```
// Go to the correct domain
driver.get("http://www.example.com");

// Now set the cookie. This one's valid for the entire domain
Cookie cookie = new Cookie("key", "value");
driver.manage().addCookie(cookie);

// And now output all the available cookies for the current URL
Set<Cookie> allCookies = driver.manage().getCookies();
for (Cookie loadedCookie : allCookies) {
    System.out.println(String.format("%s -> %s", loadedCookie.getName(), loadedCookie.getValue()));
}
```

## Next, Next Steps!

This has been a high level walkthrough of WebDriver and some of its key capabilities. You may want to look at the DesignPatterns to get some ideas about how you can reduce the pain of maintaining your tests and how to make your code more modular.