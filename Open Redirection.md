Here is a vulnerable Java code snippet that is susceptible to an Open Redirection attack: 

## 🥺 Vulnerable Code
```
String redirectUrl = request.getParameter("url");
response.sendRedirect(redirectUrl);
```
This code takes a URL parameter from an HTTP request and redirects the user to that URL. If the URL parameter is not properly validated, an attacker could specify a malicious URL that redirects the user to a phishing site or other malicious destination.

## 😎 Secure Code 
To secure this code against open redirect attacks, you can validate the URL parameter before redirecting the user. Here is an example of how you could do this:

```
import java.net.MalformedURLException;
import java.net.URL;

String redirectUrl = request.getParameter("url");
try {
URL url = new URL(redirectUrl);
String host = url.getHost();
if (host.equals("example.com") || host.endsWith(".example.com")) {
response.sendRedirect(redirectUrl);
} else {
response.sendError(HttpServletResponse.SC_BAD_REQUEST, "Invalid redirect URL");
}
} catch (MalformedURLException e) {
response.sendError(HttpServletResponse.SC_BAD_REQUEST, "Invalid redirect URL");
}
```
This code uses the java.net.URL class to parse the redirect URL and validate its hostname. It only allows redirects to URLs on the example.com domain, and sends an error response for any other URLs. This helps to prevent attackers from redirecting users to malicious sites.
