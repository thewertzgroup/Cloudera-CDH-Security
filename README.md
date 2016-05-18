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
