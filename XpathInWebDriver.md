# How XPath Works in WebDriver

At a high level, WebDriver uses a browser's native XPath capabilities wherever possible. On those browsers that don't have native XPath support, we have provided our own implementation. This can lead to some unexpected behaviour unless you are aware of the differences in the various xpath engines.

| **Driver** | **Tag and Attribute Names** | **Attribute Values** | **Native XPath Support** |
|:-----------|:----------------------------|:---------------------|:-------------------------|
| HtmlUnitDriver | Lower-cased                 | As they appear in the HTML | Yes                      |
| InternetExplorerDriver | Lower-cased                 | As they appear in the HTML | No                       |
| FirefoxDriver | Case insensitive            | As they appear in the HTML | Yes                      |

This is a little abstract, so for the following piece of HTML:

```
<input type="text" name="example" />
<INPUT type="text" name="other" />
```

The following will happen:

| **XPath expression** | **Number of Matches In** | | | |
|:---------------------|:-------------------------|:|:|:|
|                      | **HtmlUnitDriver**       | **FirefoxDriver** | **InternetExplorerDriver** |
| //input              | 1 ("example")            | 2 | 2 |
| //INPUT              | 0                        | 2 | 0 |

## Matching Implicit Attributes

Sometimes HTML do not need attributes to be explicitly declared because they will default to known values. For example, the "input" tag does not require the "type" attribute because it defaults to "text". The rule of thumb when using xpath in WebDriver is that you **should not** expect to be able to match against these implicit attributes.