Here is an example of vulnerable code for a profile update page that is susceptible to Stored Cross-site-Scripting (XSS) attacks:

# 🥺 Vulnerable Code
```
public class ProfileServlet extends HttpServlet {
protected void doPost(HttpServletRequest request, HttpServletResponse response) {
String name = request.getParameter("name");
String bio = request.getParameter("bio");
String website = request.getParameter("website");

User user = new User();
user.setName(name);
user.setBio(bio);
user.setWebsite(website);
// Save the user profile
saveUserProfile(user); 

// Redirect to the profile page 
response.sendRedirect("/profile");

}
}
```

This code allows a user to update their profile by submitting a form with their name, bio, and website. However, it does not properly sanitize the user input, making it vulnerable to XSS attacks.

An attacker could exploit this vulnerability by submitting a form with malicious JavaScript code in the “name“, “bio“, or “website” field. When the form is submitted, the malicious code would be stored in the database and displayed on the user’s profile page, potentially allowing the attacker to execute arbitrary JavaScript in the context of the user’s browser.

# 😎 Secure Code 
To secure this code against XSS attacks, we can sanitize the user input by escaping special characters in the user input. Here is an example of how the code could be modified to do this:
```
public class ProfileServlet extends HttpServlet {
protected void doPost(HttpServletRequest request, HttpServletResponse response) {

String name = request.getParameter("name");
String bio = request.getParameter("bio");
String website = request.getParameter("website");

// Sanitize user input
name = sanitizeInput(name);
bio = sanitizeInput(bio);
website = sanitizeInput(website);

User user = new User();
user.setName(name);
user.setBio(bio);
user.setWebsite(website);

// Save the user profile
saveUserProfile(user);

// Redirect to the profile page
response.sendRedirect("/profile");
}
private String sanitizeInput(String input) {
return input.replaceAll("<", "<").replaceAll(">", ">");
}
}
```
This code sanitizes the user input by replacing the “<” and “>” characters with their HTML escape codes, which prevents them from being interpreted as HTML tags. This effectively neutralizes any malicious code submitted by the user and makes the code more secure against XSS attacks.

