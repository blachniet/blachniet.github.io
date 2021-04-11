---
title: "CFSSL"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: true
---

# CFSSL

The [CFSSL README](https://github.com/cloudflare/cfssl) is really good. Here's
some additional notes.

## Change the expiry of the CA

Set the `ca.expiry` in the CSR. In the example below, we set the CA certificate
to expiry in 10 years (default is 5 years).

```json
{
  "hosts": [
    "my-ca.example.com"
  ],
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C":  "US",
      "L":  "San Francisco",
      "O":  "Internet Widgets, Inc.",
      "OU": "WWW",
      "ST": "California"
    }
  ],
  "ca": {
    "expiry": "87600h"
  }
}
```

Then, use that CSR when [generating the self-signed root CA certificate and
private key][1].

```bash
cfssl genkey -initca csr.json | cfssljson -bare ca
```

## Change the default expiry of signed certificates

Create a JSON file that looks like the following. Provide it as the `config`
flag to `cfssl gencert`.

In the example below, we set the expiry of the generated certificate to 5 years
(default is 1 year).

```json
{
  "signing": {
    "default": {
      "usages": ["signing", "key encipherment", "server auth", "client auth"],
      "expiry": "43800h"
    }
  }
}
```

```bash
cfssl gencert -ca ca.pem -ca-key ca-key.pem -config config.json csr.json
```

You can also define signing profiles in the config file and select them with the
`profile` flag. See the Configuration section in [`doc/cmd/cfssl.txt`][2] for
more information.

## Print information about a PEM-encoded certificate

This will print information about the certificate in CFSSL's standard JSON format.

```bash
cfssl certinfo -cert ca.pem
```

[1]: https://github.com/cloudflare/cfssl#generating-self-signed-root-ca-certificate-and-private-key
[2]: https://github.com/cloudflare/cfssl/blob/master/doc/cmd/cfssl.txt