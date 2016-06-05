+++
date = "2015-11-23T21:16:29+02:00"
description = "A quick reminder on how to create self-signed certificates for testing purposes."
title = "Self signed certificates for development"
tags = ["code", "security"]

+++
A quick reminder on how to create self-signed certificates for testing purposes.

### 1. Generate a Private Key

```
openssl genrsa -des3 -out server.key 1024
```

### Create CSR (Certificate Signing Request)

```
openssl req -new -sha256 -key server.key -out server.csr
```

### 3. (optional) Remove passphrase

```
cp server.key server.key.org
openssl rsa -in server.key.org -out server.key
```

### 4. Generate self-signed certificate

```
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```