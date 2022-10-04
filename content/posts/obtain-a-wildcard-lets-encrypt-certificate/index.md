---
title: "Obtain a wildcard Let's Encrypt certificate"
date: "2021-09-21"
categories:
- "site-updates"
tags:
- "lets-encrypt"
aliases:
- "/blog/obtain-a-wildcard-lets-encrypt-certificate/"
blachnietWordPressExport: true
---

Use the [manual](https://certbot.eff.org/docs/using.html?highlight=manual#manual) plugin and DNS challenge in [`certbot`](https://certbot.eff.org/docs/index.html) to obtain a wildcard [Let's Encrypt](https://letsencrypt.org/) TLS certificate.

> **Subdomains only.** You can only use this wildcard certificate on subdomains (e.g. `www.example.com`, `mail.example.com`). You cannot use it for the apex domain (e.g. `example.com`). Obtain a separate certificate for the apex domain.

```shell
$ certbot certonly --manual --preferred-challenges dns -d '*.example.com'
```

Create the TXT record as instructed by `certbot`. Before continuing, use `dig`  
or [Google's Dig tool](https://toolbox.googleapps.com/apps/dig/#TXT/) to confirm the records is applied.

```shell
$ dig txt _acme-challenge.example.com
```

Wait until `dig` shows that the record is applied. You may want to refresh/re-run it a couple times to ensure the record is updated on a few different servers.

Once you're confident the record update is applied, press `Enter` to continue the `certbot` process and continue following the instructions it provides.
