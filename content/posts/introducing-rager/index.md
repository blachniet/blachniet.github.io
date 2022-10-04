---
title: "Introducing Rager"
date: "2016-04-10"
categories:
- "site-updates"
aliases:
- "/blog/introducing-rager/"
blachnietWordPressExport: true
coverImage: "rager-lg-darkbg.png"
---

[Rager](http://rager.io) is service that notifies you when the projects you care about most publish a new release. After signing up using your GitHub account, you can start "watching" your favorite projects hosted by [GitHub](https://github.com/), [NuGet](https://www.nuget.org/) or [npm](http://npmjs.org/). Every night Rager scans for new releases. If it finds one that you are "watching", it sends you an email with a link to the new release!

This application is a side project that I've been tinkering with for a while now. I had two goals for this project.

First, I wanted to solve a simple problem that bothered me. I wanted to keep the libraries that I used in my professional and personal projects up-to-date. I hated that I had to manually go out and check the projects that I used for new releases. I found some alternative solutions that worked, but they had some quirks.

The first thing I tried was using [GitHub's watch feature](https://help.github.com/articles/watching-repositories/). The biggest downside to this approach is that I got a notifications for **any** activity on the project. That means any new pull requests and issues. All I'm really interested in is the new releases, so my inbox was flooded with notifications I didn't care about.

My second attempt was to subscribe to the atom feeds for releases. This was better, but still required me to exert the effort to actually check the feed for new updates.

I wasn't happy with either of these approaches, so I created Rager. With Rager, once I start "watching" the project, there's no extra effort involved in finding the releases, and I only get notifications for updates that I care about.

The second reason I created Rager was to learn something new. I'm trying very hard to avoid becoming a [dark matter developer](http://www.hanselman.com/blog/DarkMatterDevelopersTheUnseen99.aspx). By day, I'm your standard enterprise C# developer, but by night I like to dabble in a little bit of everything. Every once in a while I learn something in my off-time that I get to apply to my professional work, which is one of the most rewarding parts of working on side-projects.

I'm not sure that Rager provides a service that anyone else cares about, [but it at least scratches my own itch](http://blog.codinghorror.com/rule-of-three/).

If you want to keep libraries that you use up-to-date, then [check it out](http://rager.io). If you've got questions, check out the [Rager FAQ](http://rager.io/faq) or tweet [@blachniet](https://twitter.com/intent/tweet?screen_name=blachniet) or [@ragerhq](https://twitter.com/intent/tweet?screen_name=ragerhq). If you try it out, I'd love to hear what you think!
