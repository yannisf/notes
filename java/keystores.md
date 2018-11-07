# Java keystore

Java keystore is the cryptographic file format that Java uses to store keys, key pairs and certificates. The file format typically has the
extention 'JKS'.

## Keytool

Java keystores are manipulated with the `keytool` command. A desktop application `Keystore Explorer` can facilitate key store manipulation.

## Keystore and Truststore

Keystores and Truststores are practically the same with differencies only
on the conceptual and use model. The keystores should be kept safe, typically have passwords on the key and store level since they store private keys. The truststores hold certificates that are to be trusted.
Since the certificates are in principle public they do not need to have a password. Nevertheless, there must be a password on the store level to ensure that the store has not been tampered.
