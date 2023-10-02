Here is an example of Java code where Secure Cookie is not set for the Session token cookie:

## 🥺 Vulnerable Code
Here is an example of vulnerable code that does not set the “secure” flag when creating a session token cookie:

```java
String sessionToken = "abc123"; 
Cookie cookie = new Cookie("session_token", sessionToken); 
response.addCookie(cookie);
```
This code creates a new cookie and adds it to the HTTP response. However, it does not set the “secure” flag, which means that the cookie can be transmitted over an unencrypted connection. This makes it vulnerable to interception by attackers.

## 😎 Secure Code
To secure this code and set the “secure” flag on the session token cookie, you can specify the “secure” flag when creating the cookie. Here is an example of how you can do this:


```java
String sessionToken = "abc123"; 
Cookie cookie = new Cookie("session_token", sessionToken); 
cookie.setSecure(true); 
response.addCookie(cookie);
```
This code creates a new cookie and sets the “secure” flag to “true“. This ensures that the cookie is only transmitted over a secure, encrypted connection. This helps to protect the cookie from being intercepted by attackers.
