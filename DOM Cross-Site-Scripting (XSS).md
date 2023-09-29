Here is an example of vulnerable code that is susceptible to DOM-based cross-site scripting (XSS) attack:

## 🥺 Vulnerable Code
```
String userInput = request.getParameter("input");
String output = "You entered: " + userInput;
document.getElementById("output").innerHTML = output;
```
This code takes the user’s input, which is obtained from an HTTP request, and assigns it to the inner HTML of an element on the page. If the user input is not properly sanitized, it could contain malicious code, such as JavaScript.

## 😎 Secure Code
To secure this code against DOM-based XSS attacks, you can sanitize the user input by HTML-escaping it before assigning it to the inner HTML of the element. Here is an example of how you could do this:

```
import org.owasp.html.HtmlPolicyBuilder;
import org.owasp.html.PolicyFactory;

PolicyFactory policy = new HtmlPolicyBuilder()

.allowElements("a")
.allowUrlProtocols("https")
.allowAttributes("href").onElements("a")
.toFactory();

String userInput = request.getParameter("input");
String output = policy.sanitize(userInput);
document.getElementById("output").innerHTML = "You entered: " + output;
```
This code uses the OWASP HTML Sanitizer library to sanitize the user input. It allows only “<a>” elements and “href” attributes, and only allows links that use the “https” protocol. This helps to prevent malicious code from being injected into the page.
