# CredentialManager
C# wrapper around CredWrite / CredRead functions to store and retreive from Windows Credential Store
Windows OS comes equipped with a very secure robust [Credential Manager](https://technet.microsoft.com/en-us/library/jj554668.aspx) from Windows Xp onwards, and [good set of APIs] (https://msdn.microsoft.com/en-us/library/windows/desktop/aa374731(v=vs.85).aspx#credentials_management_functions) to interact with it. However .Net fraework did not provide any standard way to interact with this vault [until Windows 8.1](https://msdn.microsoft.com/en-us/library/windows/apps/windows.security.credentials.aspx).

Microsoft Peer Channel blog (WCF team) has written [a blog post](http://blogs.msdn.com/b/peerchan/archive/2005/11/01/487834.aspx) in 2005 which provided basic structure of using the WIn32 APIs for credential management in .Net.
I used their code, and improved up on it to add `PromptForCredentials` function to display a dialog to get the credentials from user.

Need: Many webservices and REST Urls use basic authentication. .Net does not have a way to generate basic auth text (username:password encoded in Base64) for the current logged in user, with their credentials.
`ICredential.GetCredential (uri, "Basic")` does not provide a way to get current user security context either as it will expose the current password in plain text. So only way to retrive Basic auth text is to propmt the user for the credentials and storing it, or assume some stored credentials in Windows store, and retreiving it.

This project provides access to all three
####1. Prompt user for Credentials
```C#
var cred = CredentialManager.PromptForCredentials ("Some Webservice", ref save, "Please provide credentials", "Credentials for service");
```            

####2. Save Credentials
```C#
var cred = new NetworkCredential ("TestUser", "Pwd");
CredentialManager.SaveCredentials ("TestSystem", cred);
```            

####3. Retreive saved Credentials
```C#
var cred = CredentialManager.GetCredentials ("TestSystem");
```            
