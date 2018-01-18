# Sign server and client certificates

## Create a key

```
 cd /root/ca
 openssl genrsa -aes256 \
      -out intermediate/private/domain.key.pem 2048
 chmod 400 intermediate/private/domain.key.pem

```

## Create a certificate

Use the private key to create a certificate signing request (CSR).

```
 cd /root/ca
 openssl req -config intermediate/openssl.cnf \
      -key intermediate/private/domain.key.pem \
      -new -sha256 -out intermediate/csr/domain.csr.pem
```

To create a certificate, use the intermediate CA to sign the CSR.

```
 cd /root/ca
 openssl ca -config intermediate/openssl.cnf \
      -extensions server_cert -days 375 -notext -md sha256 \
      -in intermediate/csr/domain.csr.pem \
      -out intermediate/certs/domain.cert.pem
 chmod 444 intermediate/certs/domain.cert.pem
```

## Verify the certificate

```
 openssl x509 -noout -text \
      -in intermediate/certs/domain.cert.pem
```

```
 openssl verify -CAfile intermediate/certs/ca-chain.cert.pem \
      intermediate/certs/domain.cert.pem
```