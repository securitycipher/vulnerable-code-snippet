Here is an example of Java code that is vulnerable to SQL Injection attack on productId parameter:

## 🥺 Vulnerable Code
```
String productId = request.getParameter("id");
String query = "SELECT * FROM products WHERE id = " + productId;

Statement st = connection.createStatement();
ResultSet rs = st.executeQuery(query);

while (rs.next()) {
String name = rs.getString("name");
String description = rs.getString("description");
// display product details
}
```
This code is vulnerable to SQL injection attacks because it is directly concatenating user input (the “productId” parameter) into the SQL query string. An attacker could input a malicious value for the “productId” parameter that modifies the SQL query in unintended ways, such as adding additional clauses or comments.

## 😎 Secure Code
To secure this code, you should use prepared statements with parameterized queries. This will prevent attackers from being able to inject malicious input into the query:

```
String productId = request.getParameter("id");
String query = "SELECT * FROM products WHERE id = ?";

PreparedStatement st = connection.prepareStatement(query);
st.setString(1, productId);
ResultSet rs = st.executeQuery();

while (rs.next()) {
String name = rs.getString("name");
String description = rs.getString("description");
// display product details
}
```
This code will safely escape any special characters in the “productId” parameter, preventing attackers from injecting malicious input into the query.
