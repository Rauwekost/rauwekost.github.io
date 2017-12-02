---
author: "Robert den Harink"
date: 2015-11-23
title: Self signed certificates
subtitle: For development purposes only!
tags: [Code, Security]
categories: [development, programming]
colour: green
---

A quick reminder on how to create self-signed certificates for testing purposes.

### Generate a private key.
```bash
openssl genrsa -des3 -out server.key 1024
```

### Create CSR (Certificate Signing Request).
```bash
openssl req -new -sha256 -key server.key -out server.csr
```

### Remove the passphrase (optional).
```bash
cp server.key server.key.org
openssl rsa -in server.key.org -out server.key
```

### Generate self-signed certificate
```bash
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```
