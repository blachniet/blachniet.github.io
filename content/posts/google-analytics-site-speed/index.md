---
title: "Google Analytics Site Speed for Small Websites"
date: "2015-01-25"
categories:
- "site-updates"
tags:
- "google-analytics"
aliases:
- "/blog/google-analytics-site-speed/"
blachnietWordPressExport: true
coverImage: "ga-site-speed-2.jpg"
---

If you're running a small website and using [Google Analytics](http://bit.ly/blachniet-ga), your site speed metrics graph might look empty. If you don't get a lot of traffic there will be no samples to measure site performance. This is because the default sampling rate is only 1% _(i.e. site speed metrics will only be collected for 1% of your visitors)_. This is plenty if your running a big site with tons of visitors per day, but if your running a smaller site this isn't going to be good enough.

Luckily, Google Analytics allows you to change this setting. If you're using Universal Analytics you can [set the site speed sample rate when creating your tracker](http://bit.ly/R4vl4g). In the example below I have set the tracker to sample 50% of my visitors by passing `{'siteSpeedSampleRate': 50}` in the `create` call.

```html
<script type="text/javascript">
(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
})(window,document,'script','//www.google-analytics.com/analytics.js','ga');

ga('create', 'UA-XXXX-Y', {'siteSpeedSampleRate': 50});
ga('send', 'pageview');
</script>
```

After doing this I got a significant increase in the number of samples per day, allowing me to get a better idea of how my site was performing.
