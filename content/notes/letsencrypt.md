---
title: "Let's Encrypt"
date: 2021-04-22T21:24:17-04:00
---

# Let's Encrypt

## Obtain wildcard certificate

Use the manual plugin and the DNS challenge to obtain a wildcard certificate.

```bash
certbot certonly --manual --preferred-challenges dns -d '*.example.com'
```

Create the TXT record as instructed by `certbot`. Before continuing, use `dig`
to confirm the records is applied.

```bash
dig txt _acme-challenge.example.com
```

When `dig` shows that the record is applied, continue the `certbot` process.
