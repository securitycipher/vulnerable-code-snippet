Here is an example of a Java code that does not set a Content Security Policy (CSP) header, leaving it vulnerable to injection attacks:

## 🥺 Vulnerable Code
```java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class NoCSPVulnerable extends HttpServlet {
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
// Generate response
PrintWriter out = response.getWriter();
out.println("<html><body>");
out.println("This page does not set a Content-Security-Policy header.");
out.println("</body></html>");
}
}
```
This code is vulnerable to injection attacks because it does not set a CSP header, allowing an attacker to potentially inject malicious script or other content into the page.

## 😎 Secure Code
Here is a version of the same code that sets a CSP header in a secure way:

```java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class NoCSPSecure extends HttpServlet {
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
// Secure code: The application sets a default-src CSP header to block all untrusted sources
response.setHeader("Content-Security-Policy", "default-src 'none'");

// Generate response
PrintWriter out = response.getWriter();
out.println("<html><body>");
out.println("This page sets a Content-Security-Policy header to block all untrusted sources.");
out.println("</body></html>");
}
}
```
This version of the code sets a default-src CSP header to block all untrusted sources. This helps to prevent injection attacks by ensuring that the page only loads trusted resources, and blocks any potentially malicious ones.
