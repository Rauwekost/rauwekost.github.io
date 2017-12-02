---
author: "Robert den Harink"
date: 2016-01-20
title: Nginx client certificates
subtitle: Be your own CA
tags: [programming, security]
colour: green
---
At work, we needed a way to verify client machines that were trying to reach our API's.

All the services are running behind a Nginx proxy that terminates SSL. This way we have one point of entry and one place to renew certification. So the most logical place to check for client certificates is Nginx.

The creation and management of client certificates can be quite the hassle so I wrote a makefile and some shell scripts to automate this process. You can find this on [github](https://github.com/tacitic/cca).

once you have a CA-certificate and generated one or more client certificates you can configure Nginx as follows:

```nginx
server {
...
  ssl_client_certificate /path/to/your/ca.crt;
  ssl_verify_client on; # on | off | optional
}
```
By setting `ssl_verify_client` to `on` every request must be made with the right client certificate. When setting this to `optional` all requests will go through and the  `$ssl_client_verify` parameter will be set to `SUCCESS`, `FAILED:reason` or `NONE` accordingly.

You can pass these variables to the underlying services and decide there what to do with the information.

For our use case, we had a PHP application running on `php-fpm` that allowed an optional client certificate.
```nginx
location / {
...
    fastcgi_param  VERIFIED $ssl_client_verify;
    fastcgi_param  DN $ssl_client_s_dn;
    fastcgi_param CERT $ssl_client_cert;
}
```
We pass the `VERIFIED`, `DN` and `CERT` parameters to fastcgi so we can do more validation in the application logic. This way we can turn on/off certain certificates and notify customers about renewal before the certificate is expired.
