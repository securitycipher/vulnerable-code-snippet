Here is an example of Java code that is vulnerable to Clickjacking attack.

## 🥺 Vulnerable Code
```java
import java.io.*; 
import javax.servlet.*; 
import javax.servlet.http.*; 

public class ClickjackingVulnerable extends HttpServlet { 
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { 

// Generate response 
PrintWriter out = response.getWriter(); 
out.println("<html><body>"); 
out.println("This page is vulnerable to clickjacking attacks."); 
out.println("</body></html>"); 
} 
}
```
This code is vulnerable to clickjacking attacks because it does not set any headers or use any techniques to prevent the page from being embedded in a malicious frame or iframe. An attacker could create a page with a hidden frame that overlays the victim’s page, tricking the victim into clicking on it and potentially leading to sensitive information being compromised.

## 😎 Secure Code
Here is a version of the same code that is secured against clickjacking attack:

```java
import java.io.*; 
import javax.servlet.*; 
import javax.servlet.http.*; 

public class ClickjackingSecure extends HttpServlet { 
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { 

// Secure code: The application sets the X-Frame-Options header to prevent the page from being embedded in a frame or iframe 
response.setHeader("X-Frame-Options", "DENY"); 

// Generate response PrintWriter out = response.getWriter(); 
out.println("<html><body>"); 
out.println("This page is secured against clickjacking attacks with the X-Frame-Options header."); 
out.println("</body></html>"); 
} 
}
```
This version of the code sets the X-Frame-Options header to “DENY“, which tells the browser to not allow the page to be embedded in a frame or iframe. This helps to prevent clickjacking attacks by ensuring that the page cannot be overlaid by a malicious frame.
