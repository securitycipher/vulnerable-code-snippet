Here is an example of Java code that is vulnerable to reflected Cross-Site Scripting (XSS) attacks on a search page:
## 🥺 Vulnerable Code
```java
String searchTerm = request.getParameter("term"); 
out.println("<h1>Search Results for: " + searchTerm + "</h1>");
```
This code takes the user-provided search term and reflects it back to the user in the search results header. An attacker could inject malicious JavaScript code into the search term, which would then be executed by the victim’s browser when they view the search results.

## 😎 Secure Code
To secure this code against XSS attacks, we can use the “escapeHtml4“ method from the “org.apache.commons.text.StringEscapeUtils” class to encode the search term as HTML. This will prevent the injected JavaScript code from being executed by the browser:
```java
import org.apache.commons.text.StringEscapeUtils;
...
String searchTerm = request.getParameter("term");
searchTerm = StringEscapeUtils.escapeHtml4(searchTerm);
out.println("<h1>Search Results for: " + searchTerm + "</h1>");
```
This will ensure that any HTML or JavaScript code injected into the search term is displayed as plain text, rather than being executed as code.
