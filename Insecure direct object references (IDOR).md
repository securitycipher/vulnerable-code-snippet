Here is an example of code that is vulnerable to IDOR (Insecure Direct Object Reference) vulnerability:

## 🥺 Vulnerable Code
```
// This code allows the user to view a list of accounts by specifying the account ID in the URL parameter 
String accountId = request.getParameter("accountId"); 
Account account = accountDao.getAccountById(accountId); 
response.getWriter().write(account.toString());
```
This code is vulnerable because it does not properly verify that the user has the necessary permissions to view the account with the specified ID. An attacker could manipulate the “accountId” parameter in the URL to view sensitive information for accounts that they are not authorized to access.

## 😎 Secure Code
To secure this code, we can add a check to verify that the user has the necessary permissions before displaying the account information:
```
String accountId = request.getParameter("accountId"); 
Account account = accountDao.getAccountById(accountId); 

// Check if the user has the necessary permissions to view the account 
if (user.getId().equals(account.getOwnerId()) || user.hasRole("ADMIN")) { 
response.getWriter().write(account.toString()); 
} else 
{ response.sendError(403, "Forbidden"); 
}
```
In the secure code, we added a check to see if the user’s ID matches the ID of the owner of the account, or if the user has the “ADMIN” role. If either of these conditions is true, the account information is displayed. Otherwise, a “Forbidden” error is returned to the user.
