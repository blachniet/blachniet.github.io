---
title: "Email Address in MVC 5 Google Authentication"
date: "2013-11-09"
categories:
- "site-updates"
tags:
- "asp-net"
- "asp-net-identity"
- "asp-net-mvc"
- "csharp"
- "openid"
aliases:
- "/blog/email-address-in-mvc-5-google-authentication/"
blachnietWordPressExport: true
---

I'm not proud to admit that I spent hours trying to figure out this very simple problem. The goal was simple: when a user logs into my web application using Google authentication, I want to be able to grab their email address so I can store it as part of their user profile. As expected, this is very simple.

I'm assuming you've already enabled Google authentication by uncommenting `app.UseGoogleAuthentication()` in your `Startup.Auth.cs`.

To get the email address, just hop on over to `AccountController.ExternalLoginCallback`. Here, after the user has successfully authenticated with Google, you can grab the external identity. That external identity has some default claims in it, one of which (for Google Authentication at least) is the user's email address.

```csharp
    //
    // GET: /Account/ExternalLoginCallback
    [AllowAnonymous]
    public async Task<ActionResult> ExternalLoginCallback(string returnUrl)
    {
        var loginInfo = await AuthenticationManager.GetExternalLoginInfoAsync();
        if (loginInfo == null)
        {
            return RedirectToAction("Login");
        }

        var externalIdentity = await AuthenticationManager
            .GetExternalIdentityAsync(DefaultAuthenticationTypes.ExternalCookie);

        var emailClaim = externalIdentity.Claims.FirstOrDefault(x => 
            x.Type.Equals(
                "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress", 
                StringComparison.OrdinalIgnoreCase));

        var emailAddress = emailClaim != null 
            ? emailClaim.Value 
            : null;

        // Remainder of method excluded for brevity...
```

The email address claim is identified by the `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` type. If you find that claim, it's `Value` is the user's email address.

If you find that you need other information that is not provided in the external identity by default, [John Palmer](https://github.com/johndpalm) has some great examples for adding claims on the [IdentityUserPropertiesSample](https://github.com/johndpalm/IdentityUserPropertiesSample/blob/master/MySample.MVC/App_Start/Startup.Auth.cs) GitHub project.
