Here is an example of vulnerable code that is susceptible to Brute Force on Login Page attack:

## 🥺 Vulnerable Code
```
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class LoginVulnerable extends HttpServlet {
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
String username = request.getParameter("username");
String password = request.getParameter("password");

User user = authenticate(username, password);
if (user != null) {
// Login successful, set session attribute and redirect to dashboard
request.getSession().setAttribute("user", user);
response.sendRedirect("/dashboard");
} else {
// Login failed, increment failed login count in session and show error message
Integer failedLoginCount = (Integer) request.getSession().getAttribute("failed_login_count");
if (failedLoginCount == null) {
failedLoginCount = 0;
}
failedLoginCount++;
request.getSession().setAttribute("failed_login_count", failedLogincount);
request.setAttribute("errorMessage", "Invalid username or password");
request.getRequestDispatcher("/login.jsp").forward(request, response);
}
}
}
```
This code is vulnerable to brute-force attacks because it does not have any protection against an attacker trying multiple invalid passwords. An attacker could write a script to try thousands of password combinations in a short period of time, potentially leading to the victim’s account being compromised.

## 😎 Secure Code
Here is a version of the same code that is secured against brute-force attacks:

```
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class LoginSecure extends HttpServlet {
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
String username = request.getParameter("username");
String password = request.getParameter("password");

User user = authenticate(username, password);
if (user != null) {
// Login successful, set session attribute and redirect to dashboard
request.getSession().setAttribute("user", user);
response.sendRedirect("/dashboard");
} else {
// Login failed, increment failed login count in session and show error message
Integer failedLoginCount = (Integer) request.getSession().getAttribute("failed_login_count");
if (failedLoginCount == null) {
failedLoginCount = 0;
}
failedLoginCount++;
request.getSession().setAttribute("failed_login_count", failedLogincount);

// Secure code: If the failed login count exceeds a threshold, log the user out for a specified duration
if (failedLoginCount > 5) {
request.getSession().setMaxInactiveInterval(1800); // 30 minutes
request.setAttribute("errorMessage", "Too many failed login attempts. You have been logged out for 30 minutes.");
request.getRequestDispatcher("/login.jsp").forward(request, response);
return;
}

request.setAttribute("errorMessage", "Invalid username or password");
request.getRequestDispatcher("/login.jsp").forward(request, response);
}
}
}
```
This version of the code includes a check for the number of failed login attempts, and if the threshold is exceeded, the user’s session is set to expire in 30 minutes. This helps to prevent an attacker from trying multiple invalid password combinations and potentially compromising the victim’s account.
