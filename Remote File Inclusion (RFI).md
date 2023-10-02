Here is an example of Java code that is vulnerable to Remote File Inclusion (RFI) attack:

## 🥺 Vulnerable Code
```java
import java.io.*; 
import java.net.*; 

public class RFI { 
public static void main(String[] args) throws Exception { 

// Vulnerable code: URL is not sanitized and is directly included in the program 
URL url = new URL(args[0]); // args[0] can be manipulated by attacker 
BufferedReader in = new BufferedReader(new InputStreamReader(url.openStream())); 

String inputLine; 
while ((inputLine = in.readLine()) != null) 
System.out.println(inputLine); 
in.close(); 
} 
}
```
## 😎 Secure Code
Here is a version of the same code that is secured against Remote File Inclusion (RFI) attack:

```java
import java.io.*; 
import java.net.*; 
import java.util.regex.*; 

public class RFI { 
public static void main(String[] args) throws Exception { 

// Secure code: URL is sanitized using regex to only allow local files 
String pattern = "^(file://)?/[A-Za-z0-9_/.-]*$"; // regex for local file URLs 
Pattern p = Pattern.compile(pattern); 
Matcher m = p.matcher(args[0]); 
if (!m.matches()) { 
System.out.println("Invalid URL"); 
return; 
} 
URL url = new URL(args[0]); 
BufferedReader in = new BufferedReader(new InputStreamReader(url.openStream())); 

String inputLine; 
while ((inputLine = in.readLine()) != null) 
System.out.println(inputLine); 
in.close(); 
} 
}
```
The line URL “url = new URL(args[0]);” was vulnerable in the original code, as it allowed an attacker to pass in a malicious URL that could be used to execute arbitrary code on the server. In the secure code, the URL is first checked against a regex pattern to ensure that it is a local file URL before it is included in the program.
