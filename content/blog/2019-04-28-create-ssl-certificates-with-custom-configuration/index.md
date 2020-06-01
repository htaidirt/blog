---
title: Create SSL Certificates with Custom Configuration
date: 2019-04-28
description: A quick post describing how to create SSL certificate with custom configuration, locally using openssl.
hero: ./images/markus-spiske-Jjue0ESkXAU-unsplash.jpg
---

Hello World!

I was recently asked to generate SSL certificates, with custom attributes (like the SAN: Subject Alt Name). After few researches, I ended up creating a configuration file that not only lists the required information, but also set defaults that matches the company information, rather than general defaults, which helps create certificates faster with less errors.

I share here this conf file, hoping that it will help developers and ops to move faster. Note that I’m using here my defaults, and you can replace by yours. Note that I did this on MacOS, and I use `openssl` command (please check the doc according to your OS, but I think Linux users should be fine too).

First, create a configuration file. Let’s say it’s named **my_openssl.conf**:

```bash
[ req ]
default_bits = 2048
default_keyfile = my_private_name.key # name of the generated keyfile
distinguished_name = req_distinguished_name
req_extensions = req_ext

[ req_distinguished_name ]
countryName = Country Name (2 letter code)
countryName_default = FR
stateOrProvinceName = State or Province Name (full name)
stateOrProvinceName_default = Paris
localityName = Locality Name (eg, city)
localityName_default = Paris
organizationName = Organization Name (eg, company)
organizationName_default = MY ORGANIZATION NAME
organizationalUnitName = Organizational Unit Name (eg, section)
organizationalUnitName_default = MY ORGANIZATIONAL UNIT NAME
commonName = Common Name (e.g. server FQDN or YOUR name)
commonName_default = NAME OF YOUR APPLICATION
emailAddress = Email Address
emailAddress_default = email@example.com

[ req_ext ]
subjectAltName = @alt_names

[alt_names]
DNS.1 = HERE PUT YOUR ALT NAME
```

In this conf file, the alt name that is optional, is now required, with a default value. For an exhaustive list of possible values of the conf file, [look here](https://github.com/openssl/openssl/blob/master/apps/openssl.cnf).

Next, to generate the keys, just run the following in your bash script:

```bash
openssl req -new -nodes -sha256 -config my_openssl.conf -out my_certificate_name.csr
```

Finally, to check everything is OK, just run the following:

```bash
openssl req -in my_certificate_name.csr -noout -text
```

You should be able to find your information and check if you didn’t miss something.

Cheers,

Hassen
