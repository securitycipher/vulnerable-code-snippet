Here is an example of vulnerable code that is susceptible to a CORS Misconfiguration:

## 🥺 Vulnerable Code
```java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class VulnerableServlet extends HttpServlet {
protected void doGet(HttpServletRequest request, HttpServletResponse response)
throws ServletException, IOException {
// This is a vulnerable code snippet with no CORS configuration
response.setHeader("Access-Control-Allow-Origin", "*"); // Allow any origin (Not recommended)
response.getWriter().write("This is a vulnerable resource.");
}
}
```
In this vulnerable code snippet, we have a servlet that does not have proper CORS configuration. It sets the Access-Control-Allow-Origin header to "*" which means it allows any origin to access the resource. This is dangerous because it allows any website, including potentially malicious ones, to make cross-origin requests to this resource, which could lead to security vulnerabilities such as CSRF attacks.

## 😎 Secure Code 
```java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class SecureServlet extends HttpServlet {
protected void doGet(HttpServletRequest request, HttpServletResponse response)
throws ServletException, IOException {
// This is a secure code snippet with proper CORS configuration
String allowedOrigin = "https://trusted-website.com";
String origin = request.getHeader("Origin");

if (allowedOrigin.equals(origin)) {
response.setHeader("Access-Control-Allow-Origin", allowedOrigin);
response.getWriter().write("This is a secure resource.");
} else {
response.setStatus(HttpServletResponse.SC_FORBIDDEN);
response.getWriter().write("Access denied. This resource can only be accessed from a trusted origin.");
}
}
}
```
In this secure code snippet, we have a servlet that implements proper CORS security. It first checks the Origin header in the incoming request to determine where the request is coming from. If the origin matches the trusted origin (https://trusted-website.com in this case), it sets the Access-Control-Allow-Origin header to that trusted origin, allowing only that specific origin to access the resource. If the origin doesn’t match the trusted origin, it returns a 403 Forbidden response, denying access to the resource.
