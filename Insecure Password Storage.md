Here is an example of vulnerable code that is susceptible to an Insecure Password Storage Practice Vulnerability :

## 🥺 Vulnerable Code
```
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class VulnerablePasswordStorage {
public static void main(String[] args) {
String username = "user123";
String plainPassword = "insecurePassword";

// Vulnerable: Storing plain text password in the database
storePasswordInsecurely(username, plainPassword);
}

public static void storePasswordInsecurely(String username, String password) {
try {
Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "username", "password");
String query = "INSERT INTO users (username, password) VALUES (?, ?)";
PreparedStatement pstmt = conn.prepareStatement(query);
pstmt.setString(1, username);
pstmt.setString(2, password); // Vulnerable: Storing plain text password

pstmt.executeUpdate();
pstmt.close();
conn.close();
} catch (SQLException e) {
e.printStackTrace();
}
}
}
```
In the vulnerable code above, the plain text password is stored directly in the database, which is highly insecure. If an attacker gains access to the database, they can easily obtain the user’s passwords.

## 😎 Secure Code
```
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.Base64;

public class SecurePasswordStorageWithSalt {
public static void main(String[] args) {
String username = "user123";
String plainPassword = "securePassword";

// Generate a random salt for the user
byte[] salt = generateSalt();

// Secure: Storing salted and hashed password in the database
storePasswordSecurely(username, salt, hashPassword(plainPassword, salt));
}

public static void storePasswordSecurely(String username, byte[] salt, String hashedPassword) {
try {
Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "username", "password");
String query = "INSERT INTO users (username, salt, password) VALUES (?, ?, ?)";
PreparedStatement pstmt = conn.prepareStatement(query);
pstmt.setString(1, username);
pstmt.setString(2, Base64.getEncoder().encodeToString(salt)); // Storing salt as a Base64 encoded string
pstmt.setString(3, hashedPassword); // Secure: Storing salted and hashed password

pstmt.executeUpdate();
pstmt.close();
conn.close();
} catch (SQLException e) {
e.printStackTrace();
}
}

public static byte[] generateSalt() {
SecureRandom random = new SecureRandom();
byte[] salt = new byte[16]; // 16 bytes (128 bits) is a common choice for salt length
random.nextBytes(salt);
return salt;
}

public static String hashPassword(String password, byte[] salt) {
try {
MessageDigest md = MessageDigest.getInstance("SHA-256");

// Combine the password and salt, then hash
md.update(salt);
byte[] hashedBytes = md.digest(password.getBytes());

// Convert bytes to hexadecimal representation
StringBuilder sb = new StringBuilder();
for (byte b : hashedBytes) {
sb.append(String.format("%02x", b));
}

return sb.toString();
} catch (NoSuchAlgorithmException e) {
e.printStackTrace();
return null;
}
}
}
```
In this Secure code, a random salt is generated for each user and stored alongside the hashed password in the database. When verifying a user’s login, you retrieve the salt from the database and use it to hash the provided password before comparing it to the stored hash. This combination of salting and hashing provides a strong defense against password attacks.
