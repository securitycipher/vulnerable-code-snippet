Here is an example of a Java code that does not set an HTTP Strict Transport Security (HSTS) header, leaving it vulnerable to man-in-the-middle attacks:

## 🥺 Vulnerable Code
```java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class NoHSTSVulnerable extends HttpServlet { 
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { 
// Generate response 
PrintWriter out = response.getWriter(); 
out.println("<html><body>"); 
out.println("This is Sample Text"); 
out.println("</body></html>"); } }
```
This code is vulnerable to man-in-the-middle attacks because it does not set an HSTS header, which tells the browser to only access the site using HTTPS. Without HSTS, an attacker could intercept the victim’s request and redirect them to a malicious site, potentially leading to sensitive information being compromised.

## 😎 Secure Code
Here is a version of the same code that sets an HSTS header in a secure way:
```java

import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class NoHSTSSecure extends HttpServlet {
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
// Secure code: The application sets an HSTS header to enforce HTTPS for the specified duration
response.setHeader("Strict-Transport-Security", "max-age=31536000");

// Generate response
PrintWriter out = response.getWriter();
out.println("<html><body>");
out.println("This page sets an HTTP Strict Transport Security header to enforce HTTPS for 1 year.");
out.println("</body></html>");
}
}
```
This version of the code sets an HSTS header with a max-age of 1 year, telling the browser to only access the site using HTTPS for the specified duration. This helps to prevent man-in-the-middle attacks by ensuring that the connection to the site is always encrypted.
