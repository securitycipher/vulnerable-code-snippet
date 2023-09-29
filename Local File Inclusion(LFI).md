Here is an example of Java code that is vulnerable to Local File Inclusion (LFI) attack:

## 🥺 Vulnerable Code
```
import java.io.FileInputStream;
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class FileInclusionServlet extends HttpServlet {
protected void doGet(HttpServletRequest request, HttpServletResponse response)
throws ServletException, IOException { 

// VULNERABLE: The value of the "file" parameter is used to construct the path to a file on the server 
String fileName = request.getParameter("file"); 
FileInputStream fis = new FileInputStream(fileName); 
ServletOutputStream outputStream = response.getOutputStream(); 

int ch; 
while ((ch = fis.read()) != -1) { 
outputStream.write(ch); 
} 
fis.close(); 
outputStream.close(); 
} 
}
```
This line of code takes the value of the “file” parameter in the request and uses it to construct the path to a file on the server. If an attacker can control the value of the “file” parameter, they can potentially access any file on the server that the Java process has permissions to read.

## 😎 Secure Code
Here is a secure version of the same code that prevents LFI attack:

```
import java.io.FileInputStream; 
import java.io.FileNotFoundException; 
import java.io.IOException; 
import javax.servlet.ServletException; 
import javax.servlet.ServletOutputStream; 
import javax.servlet.http.HttpServlet; 
import javax.servlet.http.HttpServletRequest; 
import javax.servlet.http.HttpServletResponse; 

public class FileInclusionServlet extends HttpServlet { 
protected void doGet(HttpServletRequest request, HttpServletResponse response) 
throws ServletException, IOException { 
String fileName = request.getParameter("file"); 
FileInputStream fis = null; 

try { 
// SECURE: Only allow access to files in a specific directory 
fis = new FileInputStream("/allowed/files/" + fileName); 
} catch (FileNotFoundException e) { 
// SECURE: Return a 404 error if the requested file is not found in the allowed directory 
response.sendError(HttpServletResponse.SC_NOT_FOUND); 
return; 
} 
ServletOutputStream outputStream = response.getOutputStream(); 
int ch; 
while ((ch = fis.read()) != -1) { 
outputStream.write(ch); 
} 
fis.close(); 
outputStream.close(); 
} 
}
```
This line of code only allows access to files in a specific directory, preventing an attacker from being able to access arbitrary files on the server. The secure code also returns a 404 error if the requested file is not found in the allowed directory, further enhancing security.


