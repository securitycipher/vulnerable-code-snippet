Here is an example of vulnerable code that is susceptible to an Insecure Password Reset Vulnerability :

## 🥺 Vulnerable Code
```java
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;

public class InsecurePasswordReset {

// Vulnerable method for sending reset token in the response
public Map<String, String> requestPasswordReset(String email) {
// Check if the email exists in the system (simplified for illustration)
if (userExists(email)) {
// Generate a reset token (UUID for simplicity)
String resetToken = UUID.randomUUID().toString();

// Send the reset token in the response
Map<String, String> response = new HashMap<>();
response.put("message", "Reset token sent to your email.");
response.put("resetToken", resetToken);
return response;
} else {
// User does not exist
Map<String, String> response = new HashMap<>();
response.put("message", "Email not found in our system.");
return response;
}
}

// Check if the user exists (simplified for illustration)
private boolean userExists(String email) {
// Simulated database lookup
return true; // Assume user exists for this example
}

public static void main(String[] args) {
InsecurePasswordReset resetService = new InsecurePasswordReset();
Map<String, String> response = resetService.requestPasswordReset("user@example.com");
System.out.println(response);
}
}
```
In the vulnerable code snippet, the reset token is generated and sent directly in the response. This exposes the token to potential attackers, making it easy for them to reset the password of any user if they can intercept the response. This is a significant security risk.

## 😎 Secure Code 
```java
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;

public class SecurePasswordReset {

// Secure method for requesting a password reset
public Map<String, String> requestPasswordReset(String email) {
// Check if the email exists in the system (simplified for illustration)
if (userExists(email)) {
// Generate a reset token (UUID for simplicity)
String resetToken = UUID.randomUUID().toString();

// Store the reset token securely, e.g., in a database
storeResetToken(email, resetToken);

// Send a confirmation message without exposing the token
Map<String, String> response = new HashMap<>();
response.put("message", "Reset instructions sent to your email.");
return response;
} else {
// User does not exist
Map<String, String> response = new HashMap<>();
response.put("message", "Email not found in our system.");
return response;
}
}

// Check if the user exists (simplified for illustration)
private boolean userExists(String email) {
// Simulated database lookup
return true; // Assume user exists for this example
}

// Securely store the reset token in a database
private void storeResetToken(String email, String resetToken) {
// Simulated database storage (replace with actual database code)
// Store the reset token securely associated with the user's email
}

public static void main(String[] args) {
SecurePasswordReset resetService = new SecurePasswordReset();
Map<String, String> response = resetService.requestPasswordReset("user@example.com");
System.out.println(response);
}
}
```
In the secure code snippet, we address the vulnerability by not sending the reset token in the response. Instead, we generate the token, securely store it (in a database, for example), and send a confirmation message to the user without exposing the token. This way, the reset token remains confidential and can only be used by the legitimate user to reset their password, enhancing the security of the password reset functionality.
