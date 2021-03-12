# Set up sign-in for a specific Azure Active Directory organization in Azure Active Directory B2C
## Prerequisites
Create a user flow to enable users to sign up and sign in to your application.

If you haven't already done so, register a web application, and enable ID token implicit grant.

## Register an Azure AD app

To enable sign-in for users with an Azure AD account from a specific Azure AD organization, in Azure Active Directory B2C (Azure AD B2C), you need to create an application in Azure portal. For more information, see Register an application with the Microsoft identity platform.

Sign in to the Azure portal.

Make sure you're using the directory that contains your organizational Azure AD tenant (for example, contoso.com). Select the Directory + subscription filter in the top menu, and then choose the directory that contains your Azure AD tenant.

Choose All services in the top-left corner of the Azure portal, and then search for and select App registrations.

Select New registration.

Enter a Name for your application. For example, Azure AD B2C App.

Accept the default selection of Accounts in this organizational directory only for this application.

For the Redirect URI, accept the value of Web, and enter the following URL in all lowercase letters, where your-B2C-tenant-name is replaced with the name of your Azure AD B2C tenant.

https://your-B2C-tenant-name.b2clogin.cn/your-B2C-tenant-name.partner.onmschina.cn/oauth2/authresp

Select Register. Record the Application (client) ID for use in a later step.

Select Certificates & secrets, and then select New client secret.

Enter a Description for the secret, select an expiration, and then select Add. Record the Value of the secret for use in a later step.

## Configuring optional claims
If you want to get the family_name and given_name claims from Azure AD, you can configure optional claims for your application in the Azure portal UI or application manifest. For more information, see How to provide optional claims to your Azure AD app.

Sign in to the Azure portal. Search for and select Azure Active Directory.

From the Manage section, select App registrations.

Select the application you want to configure optional claims for in the list.

From the Manage section, select Token configuration.

Select Add optional claim.

For the Token type, select ID.

Select the optional claims to add, family_name and given_name.

Click Add.

## Configure Azure AD as an identity provider
Make sure you're using the directory that contains Azure AD B2C tenant. Select the Directory + subscription filter in the top menu and choose the directory that contains your Azure AD B2C tenant.

Choose All services in the top-left corner of the Azure portal, and then search for and select Azure AD B2C.

Select Identity providers, and then select New OpenID Connect provider.

Enter a Name. For example, enter Contoso Azure AD.

For Metadata url, enter the following URL replacing {tenant} with the domain name of your Azure AD tenant:

https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration

For Client ID, enter the application ID that you previously recorded.

For Client secret, enter the client secret that you previously recorded.

For Scope, enter openid profile.

Leave the default values for Response type, and Response mode.

(Optional) For the Domain hint, enter contoso.com. For more information, see Set up direct sign-in using Azure Active Directory B2C.

Under Identity provider claims mapping, select the following claims:

User ID: oid
Display name: name
Given name: given_name
Surname: family_name
Email: preferred_username
Select Save.

## Add Azure AD identity provider to a user flow
In your Azure AD B2C tenant, select User flows.

Click the user flow that you want to add the Azure AD identity provider.

Under the Social identity providers, select Contoso Azure AD.
Select Save.

To test your policy, select Run user flow.

For Application, select the web application named testapp1 that you previously registered. The Reply URL should show https://jwt.ms.
Select the Run user flow button.

From the sign-up or sign-in page, select Contoso Azure AD to sign in with Azure AD Contoso account.

If the sign-in process is successful, your browser is redirected to https://jwt.ms, which displays the contents of the token returned by Azure AD B2C.
