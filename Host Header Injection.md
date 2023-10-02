Here is an example of vulnerable code that is susceptible to a Host Header Injection Attack :

## 🥺 Vulnerable Code
```java
import java.io.IOException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class PasswordResetServlet {
public void resetPassword(HttpServletRequest request, HttpServletResponse response) throws IOException {
String email = request.getParameter("email");
String resetLink = "https://" + request.getHeader("Host") + "/reset-password?email=" + email;

// Send password reset link to the user's email
// ...

response.sendRedirect(resetLink);
}
}
```
In the vulnerable code snippet above:

The resetPassword method takes an HTTP request and response as parameters and extracts the email parameter from the request.
It constructs a password reset link by directly using the Host header from the HTTP request. This allows an attacker to manipulate the Host header and potentially redirect the password reset link to a malicious site.
## 😎 Secure Code
```java
import java.io.IOException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class PasswordResetServlet {
private static final String APP_DOMAIN = "example.com";

public void resetPassword(HttpServletRequest request, HttpServletResponse response) throws IOException {
String email = request.getParameter("email");
String resetLink = "https://" + APP_DOMAIN + "/reset-password?email=" + email;

// Send password reset link to the user's email
// ...

response.sendRedirect(resetLink);
}
}
```
In the secure code snippet above:

We’ve introduced a constant APP_DOMAIN, which represents the legitimate domain of the application. This domain is not derived from the Host header.
Instead of using request.getHeader("Host"), we use the constant APP_DOMAIN to construct the password reset link. This ensures that the link is always generated using the expected and trusted domain, mitigating the host header injection vulnerability.
