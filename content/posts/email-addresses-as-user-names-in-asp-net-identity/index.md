---
title: "Email Addresses as User Names in ASP.NET Identity"
date: "2013-11-10"
categories:
- "site-updates"
tags:
- "asp-net"
- "asp-net-identity"
- "asp-net-mvc"
- "c"
aliases:
- "/blog/email-addresses-as-user-names-in-asp-net-identity/"
blachnietWordPressExport: true
---

It's common for web applications to use email addresses instead of user names to distinguish users. However, if you are using ASP.NET Identity, you have probably noticed that it has `UserName` built into the `IUser` interface. Since Identity assumes that this is the distinguishing field for the user, it's not crazy to think that it might be a good place to drop the email address. In order to have Identity allow an email address in this field, you will need to write a custom `IIdentityValidator`.

```
/// <summary>
/// A replacement for the <see cref="UserValidator"/> which requires that an email 
/// address be used for the <see cref="IUser.UserName"/> field.
/// </summary>
/// <typeparam name="TUser">Must be a type derived from <see cref="Microsoft.AspNet.Identity.IUser"/>.</typeparam>
/// <remarks>
/// This validator check the <see cref="IUser.UserName"/> property against the simple email regex provided at
/// http://www.regular-expressions.info/email.html. If a <see cref="UserManager"/> is provided in the constructor,
/// it will also ensure that the email address is not already being used by another account in the manager.
/// 
/// To use this validator, just set <see cref="UserManager.UserValidator"/> to a new instance of this class.
/// </remarks>
public class CustomUserValidator<TUser> : IIdentityValidator<TUser>
    where TUser : class, Microsoft.AspNet.Identity.IUser
{
    private static readonly Regex EmailRegex = new Regex(@"^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$", RegexOptions.Compiled | RegexOptions.IgnoreCase);
    private readonly UserManager<TUser> _manager;

    public CustomUserValidator()
    {
    }

    public CustomUserValidator(UserManager<TUser> manager)
    {
        _manager = manager;
    }

    public async Task<IdentityResult> ValidateAsync(TUser item)
    {
        var errors = new List<string>();
        if (!EmailRegex.IsMatch(item.UserName))
            errors.Add("Enter a valid email address.");

        if (_manager != null)
        {
            var otherAccount = await _manager.FindByNameAsync(item.UserName);
            if (otherAccount != null && otherAccount.Id != item.Id)
                errors.Add("Select a different email address. An account has already been created with this email address.");
        }

        return errors.Any()
            ? IdentityResult.Failed(errors.ToArray())
            : IdentityResult.Success;
    }
}
```

**UPDATE 03/28/2014**: As [John Holliday points out](http://blachniet.com/2013/11/10/email-addresses-as-user-names-in-asp-net-identity/#comment-1298453323), Identity 2.0 requires that the `TUser` be a reference type, so `where TUser : Microsoft.AspNet.Identity.IUser` was updated to `where TUser : class, Microsoft.AspNet.Identity.IUser`. Thanks John!

This validator ensures that the `UserName` field is set to an email address. It also optionally ensures that the email address is not being used by another account.

**UPDATE 11/17/2013**: I just found out that `ValidateAsync` is called at times other than when creating a new user, such as when adding external logins with `UserManager.AddLoginAsync`. This caused errors to occur with the original code because it thought it found duplicates. The fix this, `&& otherAccount.Id != item.Id` has been added when checking for duplicates.

In order to use this validator, just set your `UserManager.UserValidator` to a new instance of it.

```
[Authorize]
public class AccountController : Controller
{
    public AccountController()
        : this(new UserManager<ApplicationUser>(new UserStore<ApplicationUser>(new ApplicationDbContext())))
    {
        UserManager.UserValidator = new CustomUserValidator<ApplicationUser>(UserManager);
    }
```
