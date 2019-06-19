---
date: 2016-12-15
title: Pass Client Data to IdentityServer3 Views
tags: [code]
background: /assets/photos/workstate_codes/IdentityServer3views.webp
---

*This article first appeared as part of [Workstate Codes](https://www.workstate.com/blog/workstate-codes-how-to-pass-client-data-to-identityserver3-views). If you are interested in how Workstate can help you with your custom IAM needs, [reach out today](https://www.workstate.com/blog/enterprise-identity-and-access-management-at-workstate).*

IdentityServer3 provides a simple web interface for the necessary and common authentication dialogs, such as Login, Permissions Consent, and Logout Confirmation. When an application requires customization for these pages,a couple of options are provided to control output. In particular, a custom View Service provides great power and flexibility to generate custom login pages that meet a variety of requirements.

What if these authentication prompts require data from the client application? Perhaps branding needs to change based on user geolocation data, or a Login page needs to display the message of the day. The good news is that passing data into your authentication server is entirely possible, with a little bit of work.

#### Passing Values

You might expect that you could use IdentityServer's request model to pass in custom details, but unfortunately that isn't supported out of the box. Luckily, the documentation offers us the acr_values parameter. This parameter covers the optional Authentication Context Class Reference values (ProtocolMessage.AcrValues) defined as part of OpenID Connect.

According to the specification, values should be part of a space-separated string. Two values (idp and tenant) have special meaning for IdentityServer3, but others can be created at will. We pass each value as a URL-encoded key-value pair.

In the following example, we'll pass the application's version number, so it can be displayed on the login screen to the client.

```csharp
Notifications = new OpenIdConnectAuthenticationNotifications()
{
RedirectToIdentityProvider = n =>
{
    if (n.ProtocolMessage.RequestType == OpenIdConnectRequestType.AuthenticationRequest)
        {
            n.ProtocolMessage.AcrValues += " ipAddress:" + HttpUtility.UrlEncode(ipAddress);
```

Consuming these values on the Identity Server views will require a custom View Service in addition to a custom User Service.

#### Login View

The ViewService Login() method receives both a *LoginViewModel* and a *SignInMessage*. The ACR values have already been split into a collection SignInMessage.AcrValues. We can now retrieve the value and add it to the view model.

```csharp
var acrValues = message.AcrValues.ToList();
var ipAddress = HttpUtility.UrlDecode(acrValues.FirstOrDefault(a => a.StartsWith("ipAddress")));
model.Custom.ipAddress = ipAddress != null ? ipAddress.Replace("ipAddress", "") : null
```

#### Other Views

No other ViewService method receives ACR values directly. However we can access the claims for the current user by reaching into the OWIN context, even from the LoggedOut method. To create and populate claims for these values, we need a custom UserService. Within the AuthenticateLocalAsync() method, after authentication is successful, we will grab the values and add them as a custom claim.

```csharp
var acrValues = context.SignInMessage.AcrValues.ToList();
var ipAddress = HttpUtility.UrlDecode(acrValues.FirstOrDefault(a => a.StartsWith("ipAddress")));
ipAddress = ipAddress != null ? decoded.Replace("ipAddress", "") : ""; // Claim with a NULL value will throw an exception
var claims = new List<Claim>
{
    new Claim("ipAddress", ipAddress)
};
context.AuthenticateResult = new AuthenticateResult(subject, username, claims);
```

The ViewService methods can now pull these claims from the OWIN context. Note: This method does not work for Login() because the user is not yet authenticated.

```csharp
var claims = HttpContext.Current.GetOwinContext().Authentication.User.Claims.ToList();
model.Custom.ipAddress = claims.Any(c => c.Type == "ipAddress") ? claims.Single(c => c.Type == "ipAddress").Value : "";
```

#### Alternative Solutions

The acr_values solution is very handy, but limited. Passing large amounts of data or sensitive information is not recommended and in these cases, alternatives should be explored. A few possible solutions are:

* Create a custom API using Web API or any other modern framework, and call your endpoint directly from JavaScript on page load.
* Bypass the IdentityServer views entirely, instead, leveraging views on the client application. 