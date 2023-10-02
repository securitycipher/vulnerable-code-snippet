Here is an example of a vulnerable Java code that is prone to log injection attacks:

## 🥺 Vulnerable Code
```java
public void logUserAction(String username, String action) { 
// The following line is vulnerable to log injection attacks 
logger.info("User " + username + " performed action: " + action); }
```
This code is vulnerable because the “username” and “action” variables are being concatenated directly into the log message without any validation or sanitization. This means that an attacker could potentially inject malicious strings as the “username” or “action” variables in order to manipulate the log message in a way that could be harmful to the system.

## 😎 Secure Code
To secure this code against log injection attacks, we can sanitize the “username” and “action” variables before including them in the log message:

```java
public void logUserAction(String username, String action) { 

// Sanitize the input to prevent log injection attacks 
username = sanitizeInput(username); 
action = sanitizeInput(action); 

// The following line is no longer vulnerable to log injection attacks 
logger.info("User " + username + " performed action: " + action); } 

private String sanitizeInput(String input) { 
// Implement sanitization logic here 

return input; }
```
In this secure code example, the “sanitizeInput()” method can be used to sanitize the “username” and “action” variables before they are included in the log message. This helps to prevent log injection attacks by ensuring that the input strings do not contain any malicious characters or strings that could be used to manipulate the log message.
