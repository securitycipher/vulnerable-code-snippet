Here is a vulnerable Java code snippet that is susceptible to Session Fixation attack:

## 🥺 Vulnerable Code
```java
HttpSession session = request.getSession();
String sessionId = request.getParameter("sessionId");

if (sessionId != null) {
session.setId(sessionId);
}
```
This code is vulnerable to session fixation attacks because it allows an attacker to specify the session ID that should be used for the user’s session. An attacker could potentially fixate a user’s session by sending them a link with a malicious session ID, and then use that session ID to impersonate the user and gain access to their session.

## 😎 Secure Code
To secure the code against session fixation attacks, you should not allow the session ID to be specified by the client. Instead, you should generate a new, unique session ID for the user when they log in, and store it in a secure cookie that is not accessible to the client.

```java
HttpSession session = request.getSession();
String sessionId = UUID.randomUUID().toString();

session.setId(sessionId);

Cookie cookie = new Cookie("sessionId", sessionId);
cookie.setHttpOnly(true);
cookie.setSecure(true);
response.addCookie(cookie);
```
This secure code generates a new, unique session ID for the user when they log in, and stores it in a secure cookie. The session ID is not accessible to the client, which helps to prevent session fixation attacks.
