Here is an example of Java code that is vulnerable to Remote Code Execution (RCE) attack.

## 🥺 Vulnerable Code
```
import java.io.*;

public class RCE {
public static void main(String[] args) throws Exception {

// Vulnerable code: user input is directly passed to the system command
Process p = Runtime.getRuntime().exec(args[0]); // args[0] can be manipulated by attacker
BufferedReader in = new BufferedReader(new InputStreamReader(p.getInputStream()));
String line;
while ((line = in.readLine()) != null) {
System.out.println(line);
}
}
}
```
The program uses the “Runtime.getRuntime().exec()” method to execute a system command that is passed as an argument to the program. The command is passed to the program through the “args[0]” parameter, which is accessible to the attacker.

## 😎 Secure Code
Here is a version of the same code that is secured against Remote Code Execution (RCE) attack:

```
import java.io.*;
import java.util.regex.*;

public class RCE {
public static void main(String[] args) throws Exception {

// Secure code: user input is sanitized using regex to only allow approved commands
String pattern = "^[A-Za-z0-9_-]*$"; // regex for approved commands

Pattern p = Pattern.compile(pattern);
Matcher m = p.matcher(args[0]);
if (!m.matches()) {
System.out.println("Invalid command");
return;
}

Process p = Runtime.getRuntime().exec(args[0]);
BufferedReader in = new BufferedReader(new InputStreamReader(p.getInputStream()));
String line;
while ((line = in.readLine()) != null) {
System.out.println(line);
}
}
}
```
The line “Process p = Runtime.getRuntime().exec(args[0]);” was vulnerable in the original code, as it allowed an attacker to execute arbitrary code on the server. In the secure code, the user input is first checked against a regex pattern to ensure that it is an approved command before it is executed.
