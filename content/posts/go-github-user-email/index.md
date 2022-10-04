---
title: "User Email Addresses with Go-GitHub"
date: "2015-06-08"
categories:
- "site-updates"
tags:
- "github"
- "go"
aliases:
- "/blog/go-github-user-email/"
blachnietWordPressExport: true
---

I've been working on a new web app lately and decided to allow users to sign up and log in with their GitHub account. I'm using [go-github](https://github.com/google/go-github) to interact with the GitHub API and with this library there are two different ways to get a user's email address.

If you want to get the current user's primary email address, use the `Users.ListEmails()` method. This method returns all the emails associated with the GitHub user and indicates whether the email address is their `Primary` address. In order to use this method, you need to request the `user` or `user:email` [scope when acquiring the OAuth access token](https://developer.github.com/v3/oauth/#normalized-scopes).

```
// Print the current user's primary email address
emails, _, _ := githubClient.Users.ListEmails(&github.ListOptions{PerPage:10})
for _, e := range emails{
    if *e.Primary{
        fmt.Printf("Primary Email Address: %v", *e.Email)
        break;
    }
}
```

If you are using GitHub for authentication, then the above method is what you are looking for. If you don't actually require an email address and don't want to request the `user` or `user:email` scope, you can retrieve the user's publicly visible email address with `Users.Get("")`. Keep in mind that this may be `nil`, as you are not required to make an email address publicly visible in GitHub.

```
// Get the current user's public email address
user, _, _ := githubClient.Users.Get("")
if user.Email != nil{
    fmt.Printf("Public Email Address: %v", *user.Email)
}
```
