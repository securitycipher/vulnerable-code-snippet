Here is an example of code where httponly flag is not set with session token cookie:

## 🥺 Vulnerable Code
```
Cookie cookie = new Cookie("sessionToken", sessionTokenValue); 
response.addCookie(cookie);
```
## 😎 Secure Code
To secure the code, you can set the “HttpOnly” flag on the cookie to prevent it from being accessed by client-side scripts. This can help to mitigate certain types of cross-site scripting (XSS) attacks:

```
Cookie cookie = new Cookie("sessionToken", sessionTokenValue); 
cookie.setHttpOnly(true); 
response.addCookie(cookie);
```
