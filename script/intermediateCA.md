# Create the intermediate pair

## Prepare the directory
```
 mkdir /root/ca/intermediate

 cd /root/ca/intermediate
 mkdir certs crl csr newcerts private
 chmod 700 private
 touch index.txt
 echo 1000 > serial
```
## Create the intermediate key
```
 cd /root/ca
 openssl genrsa -aes256 \
      -out intermediate/private/intermediate.key.pem 4096
```

``` chmod 400 intermediate/private/intermediate.key.pem
```
## Create the intermediate certificate
```
 cd /root/ca
 openssl req -config intermediate/openssl.cnf -new -sha256 \
      -key intermediate/private/intermediate.key.pem \
      -out intermediate/csr/intermediate.csr.pem
```
```
 cd /root/ca
 openssl ca -config openssl.cnf -extensions v3_intermediate_ca \
      -days 3650 -notext -md sha256 \
      -in intermediate/csr/intermediate.csr.pem \
      -out intermediate/certs/intermediate.cert.pem 

 chmod 444 intermediate/certs/intermediate.cert.pem
```

## Verify the intermediate certificate
```
 openssl x509 -noout -text \
      -in intermediate/certs/intermediate.cert.pem
```

```
 openssl verify -CAfile certs/ca.cert.pem \
      intermediate/certs/intermediate.cert.pem
```

## Create the certificate chain file

```
 cat intermediate/certs/intermediate.cert.pem \
      certs/ca.cert.pem > intermediate/certs/ca-chain.cert.pem
 chmod 444 intermediate/certs/ca-chain.cert.pem
```