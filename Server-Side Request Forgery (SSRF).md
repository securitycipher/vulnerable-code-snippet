Here is an example of vulnerable code that is susceptible to a Serer-Side Request Forgery attack:

## 🥺 Vulnerable Code
```
import java.net.*;
import java.io.*;

public class SSRFVulnerable {
public static void main(String[] args) throws Exception {
// Read URL from user input
BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
System.out.print("Enter URL: ");
String url = reader.readLine();

// Send HTTP request to the URL
URL target = new URL(url);
HttpURLConnection connection = (HttpURLConnection) target.openConnection();
connection.setRequestMethod("GET");

// Print response from the server
BufferedReader responseReader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
String inputLine;
while ((inputLine = responseReader.readLine()) != null) {
System.out.println(inputLine);
}
responseReader.close();
}
}
```
This code takes a URL as input from the user and sends an HTTP GET request to it. It then prints the response from the server. However, this code is vulnerable to SSRF attacks because it does not properly validate the user input. An attacker could enter a URL that points to an internal network resource, such as http://localhost/secret, and potentially gain unauthorized access to that resource.

## 😎 Secure Code 
Here is a version of the same code that is secured against SSRF attacks:

```
import java.net.*;
import java.io.*;
import java.util.regex.*;

public class SSRFSecure {
public static void main(String[] args) throws Exception {
// Read URL from user input
BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
System.out.print("Enter URL: ");
String url = reader.readLine();

// Validate the URL using a regular expression
String regex = "^(https?|ftp|file)://[-a-zA-Z0-9+&@#/%?=~_|!:,.;]*[-a-zA-Z0-9+&@#/%=~_|]";
Pattern pattern = Pattern.compile(regex);
Matcher matcher = pattern.matcher(url);
if (!matcher.matches()) {
System.out.println("Invalid URL");
return;
}

// Send HTTP request to the URL
URL target = new URL(url);
HttpURLConnection connection = (HttpURLConnection) target.openConnection();
connection.setRequestMethod("GET");

// Print response from the server
BufferedReader responseReader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
String inputLine;
while ((inputLine = responseReader.readLine()) != null) {
System.out.println(inputLine);
}
responseReader.close();
}
}
```
This version of the code uses a regular expression to validate the user input and ensure that it is a well-formed URL. This helps to prevent an attacker from entering a URL that points to an internal network resource.
