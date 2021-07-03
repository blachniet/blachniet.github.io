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

{{< hint info >}}
**Google Dig**

If you don't have dig installed, you could use the
[Dig tool](https://toolbox.googleapps.com/apps/dig/#TXT/) in the [Google
Admin Toolbox](https://toolbox.googleapps.com/apps/main/).
{{< /hint >}}

## Renew manual plugin certificates

To "renew" certificates generated with the manual plugin, run the same command
that you ran to generate the certificate initially.

{{< hint info >}}
**New private key**

This approach creates a new private key as well as a new certificate.
You will need to replace both.
{{< /hint >}}

For example, to "renew" the certificate generated in
[Obtain wildcard certificate]({{< relref "#obtain-wildcard-certificate" >}}),
run the same command again.

```bash
certbot certonly --manual --preferred-challenges dns -d '*.example.com'
```
