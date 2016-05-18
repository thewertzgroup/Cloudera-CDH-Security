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
