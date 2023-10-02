Here is an example of a Java code that is vulnerable to Cross-Site Request Forgery (CSRF) attacks:

## 🥺 Vulnerable Code
```java
import java.io.*; 
import javax.servlet.*; 
import javax.servlet.http.*; 

public class CSRFVulnerable extends HttpServlet { 
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { 
// Read form data from request 
String newName = request.getParameter("new_name"); 

// Get the logged-in user from the session 
User user = (User) request.getSession().getAttribute("user"); 
if (user == null) { 
response.sendError(HttpServletResponse.SC_FORBIDDEN, "Not logged in"); 
return; 
} 

// Update the user's name 
user.setName(newName); 
updateUser(user); 
} 
}
```
This code is vulnerable to CSRF attacks because it does not include any protection against forged requests. An attacker could create a malicious website that includes a form that submits a request to this servlet to update the victim’s name. If the victim is currently logged in to the application and visits the malicious website, their browser will automatically submit the forged request and the attacker will be able to update the victim’s name.

## 😎 Secure Code
Here is a version of the same code that is secured against CSRF attacks:

```java
import java.io.*; 
import javax.servlet.*; 
import javax.servlet.http.*; 

public class CSRFSecure extends HttpServlet { 
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { 
// Check for a valid CSRF token 
String csrfToken = request.getParameter("csrf_token"); 
if (!csrfToken.equals(request.getSession().getAttribute("csrf_token"))) { 
response.sendError(HttpServletResponse.SC_FORBIDDEN, "Invalid CSRF token"); 
return; 
} 

// Read form data from request 
String newName = request.getParameter("new_name"); 

// Get the logged-in user from the session 
User user = (User) request.getSession().getAttribute("user"); 
if (user == null) { 
response.sendError(HttpServletResponse.SC_FORBIDDEN, "Not logged in"); 
return; 
} 

// Update the user's name 
user.setName(newName); 
updateUser(user); 
} 
}
```
This version of the code includes a check for a valid CSRF token in the request. The CSRF token is a random value that is stored in the user’s session and is included in any sensitive requests that are made by the application. By checking for a valid CSRF token, the application can ensure that the request is genuine and not a forged request from an attacker.
