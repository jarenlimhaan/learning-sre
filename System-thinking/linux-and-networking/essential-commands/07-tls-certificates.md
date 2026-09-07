# TLS keys, CSRs, and certificates

```bash
# Open the complete OpenSSL manual page
man openssl

# List OpenSSL's standard commands and general options
openssl help

# Show options for the req subcommand used with CSRs and certificates
openssl req -help

# Create a CSR (-new) and new 2048-bit RSA key; -nodes leaves the key unencrypted
openssl req -new -newkey rsa:2048 -nodes \
  -keyout <domain>.key -out <domain>.csr

# Read a CSR, suppress encoded output (-noout), show details (-text), and verify its signature
openssl req -in <domain>.csr -noout -text -verify

# Create a self-signed X.509 certificate valid for 365 days plus an unencrypted RSA key
openssl req -x509 -newkey rsa:2048 -nodes -days 365 \
  -keyout <domain>.key -out <domain>.crt

# Read a certificate, suppress encoded output, and print all decoded fields
openssl x509 -in <domain>.crt -noout -text

# Print only the validity dates, issuing authority, and certificate subject
openssl x509 -in <domain>.crt -noout -dates -issuer -subject

# Restrict the key to owner read/write access only
chmod 600 <domain>.key
```

Never commit or share a private key. A CSR is intended to be sent to a certificate authority; the private key is not.
