# create ca
```bash
openssl genrsa -out ca-key.pem 4096
# make sure you have set copy_extensions = copy in CA_default
openssl req -x509 -new -nodes -extensions v3_ca -key thorko-ca.key -days 3650 -out thorko-ca.pem -sha512
```

# create csr with sans
```bash
export name=sensu-1.thorko.de
export ip=10.0.0.7
openssl genrsa -out ${name}.key 4096
openssl req -new -key ${name}.key -out ${name}.csr -batch -subj "/C=DE/ST=Hessen/L=Frankfurt/O=thorko.de/OU=web/CN=${name}" -reqexts SAN -config <(cat /etc/ssl/openssl.cnf <(printf "[SAN]\nsubjectAltName=DNS:${name},IP:${ip}"))
```

# sign certificate with san
```bash
export name=sensu-1.thorko.de
export ip=10.0.0.7
openssl x509 -req -in ${name}.csr -CA thorko-ca.pem -CAkey thorko-ca.key -CAcreateserial -out ${name}.pem -days 3650 -extfile <(cat /etc/ssl/openssl.cnf <(printf "[SAN]\nsubjectAltName=DNS:${name},IP:${ip}")) -extensions SAN
```


# read csr
```bash
openssl req -in sensu-1.thorko.de.csr -text -noout
```

# sign with ca
```bash
openssl x509 -req -in sensu-1.thorko.de.csr -CA thorko-ca.pem -CAkey thorko-ca.key -CAcreateserial -out sensu-1.thorko.de.pem -days 3650
```

# check remote certificate

```bash
openssl s_client -connect website.com:443 | openssl x509 -noout -text 
```

## create tlsa record

```bash
openssl x509 -in cert.pem -pubkey -outform DER |openssl sha256
```

## validate dane record

```bash
# get dns record
dig _25._tcp.mx.thorko.de IN TLSA +short
# get cert with
openssl s_client -starttls smtp -connect mx.thorko.de:25
# copy cert to txt file and 
openssl x509 -in cert.pem -pubkey -outform DER |openssl sha256
```

