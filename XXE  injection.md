Here is a vulnerable Java code snippet that is susceptible to XXE (XML External Entity) injection attacks:

## 🥺 Vulnerable Code
```java
import java.io.StringReader;
import javax.xml.parsers.DocumentBuilderFactory;
import org.w3c.dom.Document;
import org.xml.sax.InputSource;

public class VulnerableXXE {
public static void main(String[] args) throws Exception {
String xmlData = "<?xml version=\"1.0\" encoding=\"UTF-8\"?>" +
"<user><name>John</name></user>";

// Vulnerable code: Parsing XML without disabling external entities
DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
Document document = factory.newDocumentBuilder().parse(new InputSource(new StringReader(xmlData)));

// Processing the document
String name = document.getElementsByTagName("name").item(0).getTextContent();
System.out.println("Name: " + name);
}
}
```
In this vulnerable code snippet, we parse an XML document without disabling external entity resolution. An attacker can craft a malicious XML input that references an external entity (e.g., a file on the server), leading to information disclosure or other attacks.

## 😎 Secure Code
Here is an example of a secure version of the code that properly disables external entity resolution:

```java
import java.io.StringReader;
import javax.xml.XMLConstants;
import javax.xml.parsers.DocumentBuilderFactory;
import org.w3c.dom.Document;
import org.xml.sax.InputSource;

public class SecureXXE {
public static void main(String[] args) throws Exception {
String xmlData = "<?xml version=\"1.0\" encoding=\"UTF-8\"?>" +
"<user><name>John</name></user>";

// Secure code: Disable external entity processing
DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
factory.setFeature(XMLConstants.FEATURE_SECURE_PROCESSING, true);
Document document = factory.newDocumentBuilder().parse(new InputSource(new StringReader(xmlData)));

// Processing the document
String name = document.getElementsByTagName("name").item(0).getTextContent();
System.out.println("Name: " + name);
}
}
```
In the secure code snippet, we disable external entity processing by setting the FEATURE_SECURE_PROCESSING feature to true. This prevents the parser from resolving external entities, making the code immune to XXE attacks. It’s a crucial security measure when dealing with XML data from untrusted sources.
