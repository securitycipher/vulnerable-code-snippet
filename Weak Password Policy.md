Here is a vulnerable Java code snippet that is susceptible to a Weak Password Policy :

## 🥺 Vulnerable Code
```java
import java.util.regex.*;

public class WeakPasswordPolicy {
public static boolean isValidPassword(String password) {
// Weak password policy: At least 8 characters, no requirements for special characters or numbers
String regex = "^.{8,}$";
return Pattern.matches(regex, password);
}
}
```
This code enforces a password policy that requires the password to be at least 8 characters long, but it does not have any requirements for the use of special characters or numbers. This means that passwords such as password, 12345678, and qwertyuiop would all be considered valid, which is not secure.

## 😎 Secure Code 
Here is a version of the same code that enforces a stronger password policy:

```java
import java.util.regex.*;

public class StrongPasswordPolicy {
public static boolean isValidPassword(String password) {
// Strong password policy: At least 8 characters, at least 1 special character, at least 1 number
String regex = "^(?=.*[0-9])(?=.*[!@#$%^&*])(?=\\S+$).{8,}$";
return Pattern.matches(regex, password);
}
}
```
This version of the code enforces a password policy that requires the password to be at least 8 characters long, and to contain at least 1 special character and at least 1 number. This helps to ensure that the passwords are more secure and less likely to be guessed by attackers.
