# Cloudera-CDH-Security

Enabling TLS for Cloudera Manager

Configuring TLS Encryption Only for Cloudera Manager:
http://www.cloudera.com/documentation/enterprise/latest/topics/cm_sg_tls_browser.html

Level 1: Configuring TLS Encryption for Cloudera Manager Agents:
http://www.cloudera.com/documentation/enterprise/latest/topics/cm_sg_config_tls_encr.html

Example Property Values	| Description
------------------------|------------
cmhost.sec.cloudera.com	| FQDN for Cloudera Manager Server host.
/opt/cloudera/security	| Base location for security-related files.
/opt/cloudera/security/x509	| Location for openssl key/, cert/ and cacerts/ files to be used by the Cloudera Manager Agent and Hue.
/opt/cloudera/security/jks	| Location for the Java-based keystore/ and truststore/ files for use by Cloudera Manager and Java-based cluster services.
/opt/cloudera/security/CAcerts	| Location for CA certificates (root and intermediary/subordinate CAs). One PEM file per CA in the chain is required.

Create keystore
```
keytool -genkeypair -alias cmhostname-replace -keyalg RSA -keystore \
/opt/cloudera/security/jks/cmhost-keystore.jks -keysize 2048 -dname \
"CN=cmhostname-replace.sec.cloudera.com,OU=Support,O=Cloudera,L=Denver,ST=Colorado,C=US" \
-storepass password -keypass password
```

Generate Certificate Signing Request (CSR)
```
keytool -certreq -alias cmhost \
-keystore /opt/cloudera/security/jks/cmhost-keystore.jks \
-file /opt/cloudera/security/x509/cmhost.csr -storepass password \
-keypass password
```

For self-signed certs:
Export private key from keystore:
```
keytool -importkeystore -srckeystore keystore.jks -destkeystore keystore.p12
-deststoretype PKCS12
```
```
openssl pkcs12 -in keystore.p12  -nodes -nocerts -out key.pem
```
Generate a Self-Signed Certificate from an Existing Private Key and CSR:
https://www.digitalocean.com/community/tutorials/openssl-essentials-working-with-ssl-certificates-private-keys-and-csrs
```
openssl x509 \
       -signkey domain.key \
       -in domain.csr \
       -req -days 365 -out domain.crt
```

## View Certificates
Certificate and CSR files are encoded in PEM format, which is not readily human-readable.

This section covers OpenSSL commands that will output the actual entries of PEM-encoded files.

### View CSR Entries
This command allows you to view and verify the contents of a CSR (domain.csr) in plain text:
```
openssl req -text -noout -verify -in domain.csr
```

### View Certificate Entries
This command allows you to view the contents of a certificate (domain.crt) in plain text:
```
openssl x509 -text -noout -in domain.crt
```

### Verify a Certificate was Signed by a CA
Use this command to verify that a certificate (domain.crt) was signed by a specific CA certificate (ca.crt):
```
openssl verify -verbose -CAFile ca.crt domain.crt
```
