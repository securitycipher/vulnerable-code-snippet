Here is an example of vulnerable code that is susceptible to a Unrestricted File Upload vulnerability :

## 🥺 Vulnerable Code
```
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import javax.servlet.ServletException;
import javax.servlet.annotation.MultipartConfig;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.Part;

@WebServlet("/upload")
@MultipartConfig
public class VulnerableFileUploadServlet extends HttpServlet {
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
Part filePart = request.getPart("file");
String fileName = filePart.getSubmittedFileName();

InputStream fileContent = filePart.getInputStream();
File uploadedFile = new File("/uploads/" + fileName); // Vulnerable - no validation

try (OutputStream outputStream = new FileOutputStream(uploadedFile)) {
byte[] buffer = new byte[1024];
int bytesRead;
while ((bytesRead = fileContent.read(buffer)) != -1) {
outputStream.write(buffer, 0, bytesRead);
}
}

response.getWriter().println("File uploaded successfully.");
}
}
```
In this vulnerable code, there is no validation on the file type or any checks to ensure that the uploaded file is not malicious. An attacker can upload any file, including executable scripts, which can lead to server compromise.

## 😎 Secure Code
```
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import javax.servlet.ServletException;
import javax.servlet.annotation.MultipartConfig;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.Part;

@WebServlet("/upload")
@MultipartConfig
public class SecureFileUploadServlet extends HttpServlet {
private static final String UPLOAD_DIRECTORY = "/uploads/";
private static final int MAX_FILE_SIZE = 1024 * 1024; // 1 MB

protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
Part filePart = request.getPart("file");
String fileName = filePart.getSubmittedFileName();

// Check file type (allow only image files)
if (fileName.endsWith(".jpg") || fileName.endsWith(".png") || fileName.endsWith(".gif")) {
InputStream fileContent = filePart.getInputStream();
File uploadedFile = new File(getServletContext().getRealPath("") + UPLOAD_DIRECTORY + fileName);

if (filePart.getSize() <= MAX_FILE_SIZE) {
try (OutputStream outputStream = new FileOutputStream(uploadedFile)) {
byte[] buffer = new byte[1024];
int bytesRead;
while ((bytesRead = fileContent.read(buffer)) != -1) {
outputStream.write(buffer, 0, bytesRead);
}
}
response.getWriter().println("File uploaded successfully.");
} else {
response.getWriter().println("File size exceeds the limit.");
}
} else {
response.getWriter().println("Invalid file type.");
}
}
}
```
In this secure code, the uploaded file is checked to ensure it is an image file (jpg, png, or gif), and there is a size limit to prevent excessively large files. This restricts the types of files that can be uploaded and helps mitigate the unrestricted file upload vulnerability. Additionally, the uploaded file is stored in a secure directory and not executed as code.
