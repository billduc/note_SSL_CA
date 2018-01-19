# CA certificate

1. Create a CA key 
2. Create CA certificate and sign it with the private key from step 1
3. Create the server key 
4. Create a CA certificate sign request using the key from step 3
5. Use the CA certificate from step 2 to sign the request from step 4

![Image of Yaktocat](https://mcuoneclipse.files.wordpress.com/2017/04/tls-handshaking-with-certificates-and-keys.png)

### Create CA Key Pair

```
openssl genrsa -des3 -out ca.key 2048
```

### Create CA certificate and sign it with the private key from step 1

```
openssl req -new -x509 -days 3650 -key ca.key -out ca.crt
```

### Create the server key

```
openssl genrsa -out server.key 2048
```

### Create Certificate Request from CA

```
openssl req -new -out CSRserver.csr -key server.key
```

### Verify and Sign the Certificate Request

```
openssl x509 -req -in CSRserver.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 3650
```