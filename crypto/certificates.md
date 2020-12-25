# Certificates and keys

## Create a Root CA certificate

- Generate a keypair
- Assign a CN
- Add extension Basic constraints: This subject is a CA
- Add extension Key usage: Certificate signing
- Export the public key
- Import the public key in the browser's CA certificates

## Obtain a CA signed server certificate

- Generate a keypair
- Assign as CN the domain name of the server
- Create a CSR
- Use the CA key created in the previous step to sign the CSR
- Import the signed CSR in the keystore
- This keystore should be used on the server as keystore

## Create trusted client certificates

For the purpose of this example the client certificates will be
signed by the CA created previously.

- Generate a keypair
- Assign a CN
- Create a CSR
- Use the CA key created in the previous step to sign the CSR
- Import the signed CSR in the keystore
- Import this keystore in the browser's

### Points to take into account when creating server side certificates

* Select adequate key size. When creating RSA key, use at least 2048 bits.
* Select a strong hashing algorithm. At least SHA256.
* Use at most 2 years expiration for the certificate.
* Use SAN and include there all valid DNS/IP. CN is not adequate.
* ExtendedKeyUsage should note only Web server TLS
