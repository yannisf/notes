# Command line mail sending

In order to send an email you need to transmit your mail using the SMTP protocol. To this end, you might choose to use an email client (Thunderbird) and contact directly a remote SMTP server, use a local SMTP daemon (postfix) to send your mail, or deploy a mini SMTP helper program to do the job. In order to use an SMTP helper, you will have to connect to a remote SMTP that will relay your email. Let this remote SMTP be Google's GMail, and lets assume as well that your Google account is protected with two factor authentication.

This guide is accurate as of Jul 2019.

## Summary

- Get an application password from Google
- Install and configure `msmtp`
- Send emails from command line

## Application password

- Log into your Google account
- Security/Signing in to Google/App passwords
- Generate and save locally the password token

## Install and configure `msmtp`

    sudo apt-get install msmtp

Create the configuration  in your home directory:

    vi ~/.msmtprc

and insert the following content, using your username and password acquired from previous step:

```
defaults
auth           on
tls            on
tls_trust_file /etc/ssl/certs/ca-certificates.crt
logfile        ~/.msmtp.log

account        gmail
host           smtp.gmail.com
port           587
from           <username>@gmail.com
user           <username>
password       <app_password>

account default: gmail
```

Then secure the configuration file like so:

    chmod 600 .msmtprc

To test the service sent a message:

    echo "Hello" | mssmtp <email>

## Send email

You might have noticed that the test command above just sends a rudimentary message. You might want to send more complete email, with subject, attachments etc.

Install a couple of more packages:

    sudo apt install msmtp-mta mailutils

Send email, possibly with attachments using the `mail` command:

    mail -s "Subject" -A attachment1 -A attachment2 user@example.com < content.txt

## Sending one liner emails with `curl`

Nevertheless, if all you want is to send one liner messages it is far more easier to do this throught `curl` as shown below:

    echo "This is the message" | curl --mail-rcpt user@example.com -T - smtps://user:password@smtp.gmail.com:465
