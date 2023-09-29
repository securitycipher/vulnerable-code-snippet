Here is an example of vulnerable code that is susceptible to a Java Deserialization :

## 🥺 Vulnerable Code
```
import java.io.*;

public class VulnerableDeserialization {
public static void main(String[] args) {
try {
// Deserialize data from a file
FileInputStream fileIn = new FileInputStream("data.ser");
ObjectInputStream in = new ObjectInputStream(fileIn);

// Deserialize the object and cast it
Object obj = in.readObject(); // Vulnerable point

// Do something with the deserialized object
// For a real attack, an attacker could place malicious code here.
System.out.println("Deserialized object: " + obj.toString());

in.close();
fileIn.close();
} catch (IOException | ClassNotFoundException e) {
e.printStackTrace();
}
}
}
```
In the vulnerable code above, we are deserializing an object from a file using ObjectInputStream. The problem is that this code does not validate or sanitize the data being deserialized. An attacker could potentially craft a malicious serialized object and inject harmful code or exploit the application.

## 😎 Secure Code
```
import java.io.*;

public class SecureDeserialization {
public static void main(String[] args) {
try {
// Deserialize data from a file
FileInputStream fileIn = new FileInputStream("data.ser");
ObjectInputStream in = new ObjectInputStream(fileIn);

// Deserialize the object and cast it safely
Object obj = in.readObject();

// Perform type checking to ensure the deserialized object is of the expected type
if (obj instanceof SomeClass) {
SomeClass secureObject = (SomeClass) obj;
// Use the deserialized object safely
System.out.println("Deserialized object: " + secureObject.toString());
} else {
System.out.println("Invalid object type. Aborting deserialization.");
}

in.close();
fileIn.close();
} catch (IOException | ClassNotFoundException e) {
e.printStackTrace();
}
}
}
```
In the secure code, we perform the following security measures:

We ensure that the deserialized object is of the expected type (SomeClass) by using the instanceof operator.
We cast the deserialized object to the expected type only if the type check is successful.
If the object’s type is not as expected, we abort the deserialization process.
