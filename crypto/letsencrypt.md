# Let's Encrypt

[Let's Encrypt](https://letsencrypt.org/) is a CA that provides certificates for free, provided you can
prove the ownership of a domain.

## Proving ownership

There are various ways of proving ownership, but a really quick procedure that just works is going to be shown here.

### Assumptions

* You have a publicly resolvable domain name
* You have shell access to the machine that the domain is resolved on
* The machine is reachable from the Internet through port 80
* A web server is running on the machine on port 80
* You have write access to the web server webroot directory
* You are running Ubuntu

## Install Certbot

[Certbot](https://certbot.eff.org/) is the application that will act as an agent between your server and Let's Encrypt. To install it:

```
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install certbot
```

## Start a web server

If a webserver is not available, you can start one from Python:

```
$ mkdir tmp
$ cd tmp
$ sudo python -m SimpleHTTPServer 80
```
**TIP**: Ensure that you create an empty directory to start the server from as to inadvertely expose server files.

## Get the certificate

Run the command:

```bash
$ sudo certbot certonly --webroot -w tmp -d mydomain.com
```

**tmp**: this is the root folder of the web server started previously
**mydomain.com**: this is the domain name you own, and want to create a certificate for.

After answering a couple of questions the certificate is ready. The following notes are printed and contain important information:

```txt
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator webroot, Installer None
Cert is due for renewal, auto-renewing...
Renewing an existing certificate
Performing the following challenges:
http-01 challenge for frlab.eu
Using the webroot path /var/www/html for all unmatched domains.
Waiting for verification...
Cleaning up challenges

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/frlab.eu/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/frlab.eu/privkey.pem
   Your cert will expire on 2019-07-28. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

## Files generated

* cert1.pem
* chain1.pem
* fullchain1.pem
* privkey1.pem

## Create a PKCS#12 file of key and certificate

```bash
$ openssl pkcs12 \
    -export \
    -in fullchain1.pem \
    -inkey privkey1.pem \
    -out server.p12 \
    -name mydomain
```
## Create a JKS out of PKCS#12

```
$ keytool -importkeystore \
  -deststorepass [changeit] \
  -destkeypass [changeit] \
  -destkeystore server.jks \
  -srckeystore server.p12 \
  -srcstoretype PKCS12 \
  -srcstorepass some-password \
  -alias [some-alias]
