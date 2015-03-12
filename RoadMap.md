# WebDriver RoadMap

The following issues need to be resolved before the final release:

| **Issue** | **Summary** | **HtmlUnitDriver Progress** | **FirefoxDriver Progress** | **InternetExplorerDriver Progress** | **ChromeDriver Progress** |
|:----------|:------------|:----------------------------|:---------------------------|:------------------------------------|:--------------------------|
| [27](http://code.google.com/p/webdriver/issues/detail?id=27)  | Handle alerts in Javascript-enabled browsers | n/a                         | Started                    | Started                             | Not Started               |
| [32](http://code.google.com/p/webdriver/issues/detail?id=32) | User guide  | Started                     | | | |
| [34](http://code.google.com/p/webdriver/issues/detail?id=34)  | Support HTTP Basic and Digest Authentication | Not Started                 | | | |
| [35](http://code.google.com/p/webdriver/issues/detail?id=35)  | [Selenium](http://www.openqa.org/selenium-rc) emulation | Done for Java and C#        | | | |
| [36](http://code.google.com/p/webdriver/issues/detail?id=36) | Support for drag and drop behaviour | n/a                         | Done                       | Done                                | Started                   |
| none      | Example tests | Not Started                 | | | |

A final release will be made once these are implemented in Firefox, IE and at least one webkit-based browser.

## The Future

The following are also planned:

  * **JsonWireProtocol** --- The formalisation of the current RemoteWebDriver wire protocol in [JSON](http://www.json.org/).