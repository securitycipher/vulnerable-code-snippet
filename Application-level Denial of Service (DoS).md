Here is an example of vulnerable code that is susceptible to a Application-level Denial of Service (DoS) Vulnerability:

## 🥺 Vulnerable Code 
```
import java.io.*;
import java.net.*;

public class VulnerableDoSApp {
public static void main(String[] args) {
try {
ServerSocket serverSocket = new ServerSocket(8080);
while (true) {
Socket socket = serverSocket.accept();
Thread thread = new Thread(new RequestHandler(socket));
thread.start();
}
} catch (IOException e) {
e.printStackTrace();
}
}
}

class RequestHandler implements Runnable {
private final Socket socket;

public RequestHandler(Socket socket) {
this.socket = socket;
}

public void run() {
try {
// Simulate some heavy processing
Thread.sleep(1000);

// Read the request and send a response
BufferedReader reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
PrintWriter writer = new PrintWriter(socket.getOutputStream(), true);
String request = reader.readLine();
writer.println("Response to: " + request);

// Close the socket
socket.close();
} catch (IOException | InterruptedException e) {
e.printStackTrace();
}
}
}
```

In this vulnerable code, we have a simple server that accepts incoming connections and processes requests using a new thread for each connection. The vulnerability here is that it creates a new thread for each incoming connection without any rate limiting or resource management, which can lead to resource exhaustion if an attacker floods the server with connections.

## 😎 Secure Code 
```
import java.io.*;
import java.net.*;
import java.util.concurrent.*;

public class SecureDoSApp {
private static final int MAX_THREADS = 10;
private static final ExecutorService executor = Executors.newFixedThreadPool(MAX_THREADS);

public static void main(String[] args) {
try {
ServerSocket serverSocket = new ServerSocket(8080);
while (true) {
Socket socket = serverSocket.accept();
executor.execute(new RequestHandler(socket));
}
} catch (IOException e) {
e.printStackTrace();
}
}
}

class RequestHandler implements Runnable {
private final Socket socket;

public RequestHandler(Socket socket) {
this.socket = socket;
}

public void run() {
try {
// Simulate some processing
Thread.sleep(1000);

// Read the request and send a response
BufferedReader reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
PrintWriter writer = new PrintWriter(socket.getOutputStream(), true);
String request = reader.readLine();
writer.println("Response to: " + request);

// Close the socket
socket.close();
} catch (IOException | InterruptedException e) {
e.printStackTrace();
}
}
}
```
In this secure code, we limit the number of concurrent threads using a thread pool (ExecutorService) with a fixed number of threads (MAX_THREADS). This prevents an attacker from overwhelming the server with too many simultaneous connections, thus mitigating the application-level DoS vulnerability. The server can only handle a limited number of connections concurrently, ensuring that resources are not exhausted.
