# SSL ciphers and protocols

The most common implementation of SSL is OpenSSL. This document will try to
explain a few SSL concepts and show useful commands to test and troubleshoot
SSL connections.

## Ciphers

A cipher in SSL has a name. A typical name is `ECDHE-RSA-AES256-GCM-SHA384`. The name is consisted of parts, that in order signify the following:

* Key exchange algorithm (Kx)
* Authentication (Au)
* Encryption algorithm (Enc)
* MAC digest algorithm (Mac)

So for the sample cipher one can deduce that:

* For key exchange it uses _ECDHE_
* For authentication _RSA_
* For encryption _AES265-GCM_ (GCM ensures block integrity)
* For MAC _SHA384_

Lists ciphers supported by OpenSSL:

    $ openssl ciphers
    ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:SRP-DSS-AES-256-CBC-SHA:SRP-RSA-AES-256-CBC-SHA:SRP-AES-256-CBC-SHA:DH-DSS-AES256-GCM-SHA384:DHE-DSS-AES256-GCM-SHA384...

## Protocols

Protocols define how an encrypted connectio  is initiated. Typically,
one would find the following protocols:

* **SSLv2** (insecure and obsoleted)
* **SSLv3** (insecure and obsoleted)
* **TLSv1.0** (has vulnerabilities and should be avoided)
* **TLSv1.1** (has vulnerabilities and should be avoided)
* **TLSv1.2** (secure and widely implemented)
* **TLSv1.3** (secure but not widely implemented)

Not all protocols support all ciphers. By requesting a ciphers verbose output,
the protocols are printed as well:

    $yannis@ouranos:~$ openssl ciphers -v
    ECDHE-RSA-AES256-GCM-SHA384     TLSv1.2 Kx=ECDH Au=RSA      Enc=AESGCM(256) Mac=AEAD
    ECDHE-ECDSA-AES256-GCM-SHA384   TLSv1.2 Kx=ECDH Au=ECDSA    Enc=AESGCM(256) Mac=AEAD
    ECDHE-RSA-AES256-SHA384         TLSv1.2 Kx=ECDH Au=RSA      Enc=AES(256)    Mac=SHA384
    ECDHE-ECDSA-AES256-SHA384       TLSv1.2 Kx=ECDH Au=ECDSA    Enc=AES(256)    Mac=SHA384
    ECDHE-RSA-AES256-SHA            SSLv3   Kx=ECDH Au=RSA      Enc=AES(256)    Mac=SHA1
    ECDHE-ECDSA-AES256-SHA          SSLv3   Kx=ECDH Au=ECDSA    Enc=AES(256)    Mac=SHA1
    ...

## Connecting to SSL servers

    $ openssl s_client -connect host:port {protocol} {cipher}

* {protocol}:= [-ssl2|-ssl3|-tls1|-tls1_1|-tls1_2]
* {cipher}:= -cipher _cipher_name_

To probe a server for accepted protocols and ciphers, the _Nmap_ tool has a
really nifty script/command to do so:

    nmap --script ssl-enum-ciphers

## Useful resources
* OpenSSL ciphers manual page (_man ciphers_)
* [List ciphers used by JVM](https://confluence.atlassian.com/stashkb/list-ciphers-used-by-jvm-679609085.html)
* [TLS Cipher String Cheat Sheet](https://www.owasp.org/index.php/TLS_Cipher_String_Cheat_Sheet)
* [Check supported ciphers script](https://superuser.com/questions/109213/how-do-i-list-the-ssl-tls-cipher-suites-a-particular-website-offers)
* [SSL server test](https://www.ssllabs.com/ssltest/)
* [SSL client test](https://www.ssllabs.com/ssltest/viewMyClient.html)
