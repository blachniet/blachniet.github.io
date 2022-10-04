---
title: "A New Look for 2015"
date: "2015-02-02"
categories:
- "site-updates"
aliases:
- "/blog/a-new-look-for-2015/"
blachnietWordPressExport: true
---

I have wanted to revamp my website for close to a year now. In that time I've looked into different blogging engines and tried creating new themes, but I didn't settle on a path forward until a couple weeks ago.

### Engine

At one point I considered moving to a statically generated site since they were all the rage. I tried out [Jekyll](http://jekyllrb.com/) and [Middleman](https://middlemanapp.com/). I liked both and even wrote up a post on using [Zurb's Foundation with Middleman](http://blachniet.com/blog/middleman-foundation/). However, I decided that by sticking with WordPress I could focus more on creating content.

### Markdown

Something that I really like about the static site generators is their first-class support for Markdown. I routinely write Markdown, so I felt that if I could generate my posts in Markdown, I would have one less hurdle for generating content. Before, I was using the [WP-Markdown](https://wordpress.org/plugins/wp-markdown/) plugin, but I don't like that it doesn't retain the original Markdown content. Instead, it generates and saves the HTML when the post is saved, then converts it back to Markdown when editing the post. This probably works for most people, but I was having trouble with some of my code blocks getting messed up during the conversions. I decided to start using the Markdown feature in the [Jetpack](https://wordpress.org/plugins/jetpack/) plugin. This plugin retains the original Markdown, and I've been very happy with it so far. This plugin also works well [highlight.js](https://highlightjs.org/), which I'm using for syntax highlight on the site.

### Planning

The biggest problem I have had so far with maintaining my blog is generating posts. Over 3 years I have only published 31 posts, and only 2 in 2014. So, I decided to be more deliberate about pulling together post ideas and notes. I created a [Trello](https://trello.com) board where I'm maintaining a list of post ideas and collecting notes for posts that I'm working on. I use other Trello boards for other purposes and have had a lot of success with them, so I'm hopeful that this will improve my content creation process.

### Design

I really liked the new [Twenty Fifteen](https://wordpress.org/themes/twentyfifteen) theme for WordPress, which is why I decided to base my new theme on it. I created the [Block 15](https://github.com/blachniet/block15) child theme to add some customizations for my site. It has a lot of stuff hardcoded specifically for my website, but I decided to open source it on GitHub in case it could help anyone else interested in creating their own child theme.

### Discussion

I decided to drop my use of [Disqus](https://disqus.com/) with the new theme. The Disqus thread always looked so ugly on the site and significantly increased the page load time. Instead, I added a Twitter link at the bottom of each post, encouraging readers to start a conversation about the post on Twitter. In addition to avoiding an ugly, long discussion thread on posts, this also increases my site's overall visibility. Some users that have never been to my [blachniet.com](http://blachniet.com) might observe a conversation on Twitter, and decide to check out my site as a result.

So, here's to better year of blogging in 2015.
