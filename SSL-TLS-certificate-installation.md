Certificate can be imported in AWS Certificate manager. 

1. Convert `pfx` file to `pem` fromat. Commands described later in the text.
2. Paste first certificate block `BEGIN CERTIFICATE` into the first field in form.
3. Copy `BEGIN RSA PRIVATE KEY` and paste into the second one. 
4. If you have more than one `BEGIN CERTIFICATE` block, then copy/paste rest of the certificates into the third field in the form. 

# To convert a certificate bundle from PKCS#12 (PFX) to PEM

> Use the OpenSSL pkcs12 command, as in the following example. In the following example command, replace CertificateBundle.pfx with the name of the file that contains your PKCS#12-encoded certificate bundle. Replace CertificateBundle.pem with the preferred name of the output file to contain the PEM-encoded certificate bundle.

```
$ openssl pkcs12 -in CertificateBundle.pfx -out CertificateBundle.pem -nodes
```

## Example PEM-encoded certificate

```
-----BEGIN CERTIFICATE-----
Base64-encoded certificate
-----END CERTIFICATE-----
```

## Example PEM-encoded, unencrypted private key

```
-----BEGIN RSA PRIVATE KEY-----
Base64-encoded private key
-----END RSA PRIVATE KEY-----
```

## Example PEM-encoded certificate chain

A certificate chain contains one or more certificates. The following example contains three certificates, but your certificate chain might contain more or fewer.

```
-----BEGIN CERTIFICATE-----
Base64-encoded certificate
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
Base64-encoded certificate
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
Base64-encoded certificate
-----END CERTIFICATE-----
```

AWS documentation for certs: [Working with Server Certificates](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_server-certs.html)