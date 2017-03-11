# NAME

SSL\_CTX\_use\_certificate, SSL\_CTX\_use\_certificate\_ASN1,
SSL\_CTX\_use\_certificate\_file, SSL\_use\_certificate, SSL\_use\_certificate\_ASN1,
SSL\_use\_certificate\_file, SSL\_CTX\_use\_certificate\_chain\_file,
SSL\_use\_certificate\_chain\_file,
SSL\_CTX\_use\_PrivateKey, SSL\_CTX\_use\_PrivateKey\_ASN1,
SSL\_CTX\_use\_PrivateKey\_file, SSL\_CTX\_use\_RSAPrivateKey,
SSL\_CTX\_use\_RSAPrivateKey\_ASN1, SSL\_CTX\_use\_RSAPrivateKey\_file,
SSL\_use\_PrivateKey\_file, SSL\_use\_PrivateKey\_ASN1, SSL\_use\_PrivateKey,
SSL\_use\_RSAPrivateKey, SSL\_use\_RSAPrivateKey\_ASN1,
SSL\_use\_RSAPrivateKey\_file, SSL\_CTX\_check\_private\_key, SSL\_check\_private\_key
\- load certificate and key data

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CTX_use_certificate(SSL_CTX *ctx, X509 *x);
    int SSL_CTX_use_certificate_ASN1(SSL_CTX *ctx, int len, unsigned char *d);
    int SSL_CTX_use_certificate_file(SSL_CTX *ctx, const char *file, int type);
    int SSL_use_certificate(SSL *ssl, X509 *x);
    int SSL_use_certificate_ASN1(SSL *ssl, unsigned char *d, int len);
    int SSL_use_certificate_file(SSL *ssl, const char *file, int type);

    int SSL_CTX_use_certificate_chain_file(SSL_CTX *ctx, const char *file);
    int SSL_use_certificate_chain_file(SSL *ssl, const char *file);

    int SSL_CTX_use_PrivateKey(SSL_CTX *ctx, EVP_PKEY *pkey);
    int SSL_CTX_use_PrivateKey_ASN1(int pk, SSL_CTX *ctx, unsigned char *d,
                                    long len);
    int SSL_CTX_use_PrivateKey_file(SSL_CTX *ctx, const char *file, int type);
    int SSL_CTX_use_RSAPrivateKey(SSL_CTX *ctx, RSA *rsa);
    int SSL_CTX_use_RSAPrivateKey_ASN1(SSL_CTX *ctx, unsigned char *d, long len);
    int SSL_CTX_use_RSAPrivateKey_file(SSL_CTX *ctx, const char *file, int type);
    int SSL_use_PrivateKey(SSL *ssl, EVP_PKEY *pkey);
    int SSL_use_PrivateKey_ASN1(int pk, SSL *ssl, unsigned char *d, long len);
    int SSL_use_PrivateKey_file(SSL *ssl, const char *file, int type);
    int SSL_use_RSAPrivateKey(SSL *ssl, RSA *rsa);
    int SSL_use_RSAPrivateKey_ASN1(SSL *ssl, unsigned char *d, long len);
    int SSL_use_RSAPrivateKey_file(SSL *ssl, const char *file, int type);

    int SSL_CTX_check_private_key(const SSL_CTX *ctx);
    int SSL_check_private_key(const SSL *ssl);

# DESCRIPTION

These functions load the certificates and private keys into the SSL\_CTX
or SSL object, respectively.

The SSL\_CTX\_\* class of functions loads the certificates and keys into the
SSL\_CTX object **ctx**. The information is passed to SSL objects **ssl**
created from **ctx** with [SSL\_new(3)](http://man.he.net/man3/SSL_new) by copying, so that
changes applied to **ctx** do not propagate to already existing SSL objects.

The SSL\_\* class of functions only loads certificates and keys into a
specific SSL object. The specific information is kept, when
[SSL\_clear(3)](http://man.he.net/man3/SSL_clear) is called for this SSL object.

SSL\_CTX\_use\_certificate() loads the certificate **x** into **ctx**,
SSL\_use\_certificate() loads **x** into **ssl**. The rest of the
certificates needed to form the complete certificate chain can be
specified using the
[SSL\_CTX\_add\_extra\_chain\_cert(3)](http://man.he.net/man3/SSL_CTX_add_extra_chain_cert)
function.

SSL\_CTX\_use\_certificate\_ASN1() loads the ASN1 encoded certificate from
the memory location **d** (with length **len**) into **ctx**,
SSL\_use\_certificate\_ASN1() loads the ASN1 encoded certificate into **ssl**.

SSL\_CTX\_use\_certificate\_file() loads the first certificate stored in **file**
into **ctx**. The formatting **type** of the certificate must be specified
from the known types SSL\_FILETYPE\_PEM, SSL\_FILETYPE\_ASN1.
SSL\_use\_certificate\_file() loads the certificate from **file** into **ssl**.
See the NOTES section on why SSL\_CTX\_use\_certificate\_chain\_file()
should be preferred.

SSL\_CTX\_use\_certificate\_chain\_file() loads a certificate chain from
**file** into **ctx**. The certificates must be in PEM format and must
be sorted starting with the subject's certificate (actual client or server
certificate), followed by intermediate CA certificates if applicable, and
ending at the highest level (root) CA. SSL\_use\_certificate\_chain\_file() is
similar except it loads the certificate chain into **ssl**.

SSL\_CTX\_use\_PrivateKey() adds **pkey** as private key to **ctx**.
SSL\_CTX\_use\_RSAPrivateKey() adds the private key **rsa** of type RSA
to **ctx**. SSL\_use\_PrivateKey() adds **pkey** as private key to **ssl**;
SSL\_use\_RSAPrivateKey() adds **rsa** as private key of type RSA to **ssl**.
If a certificate has already been set and the private does not belong
to the certificate an error is returned. To change a certificate, private
key pair the new certificate needs to be set with SSL\_use\_certificate()
or SSL\_CTX\_use\_certificate() before setting the private key with
SSL\_CTX\_use\_PrivateKey() or SSL\_use\_PrivateKey().

SSL\_CTX\_use\_PrivateKey\_ASN1() adds the private key of type **pk**
stored at memory location **d** (length **len**) to **ctx**.
SSL\_CTX\_use\_RSAPrivateKey\_ASN1() adds the private key of type RSA
stored at memory location **d** (length **len**) to **ctx**.
SSL\_use\_PrivateKey\_ASN1() and SSL\_use\_RSAPrivateKey\_ASN1() add the private
key to **ssl**.

SSL\_CTX\_use\_PrivateKey\_file() adds the first private key found in
**file** to **ctx**. The formatting **type** of the certificate must be specified
from the known types SSL\_FILETYPE\_PEM, SSL\_FILETYPE\_ASN1.
SSL\_CTX\_use\_RSAPrivateKey\_file() adds the first private RSA key found in
**file** to **ctx**. SSL\_use\_PrivateKey\_file() adds the first private key found
in **file** to **ssl**; SSL\_use\_RSAPrivateKey\_file() adds the first private
RSA key found to **ssl**.

SSL\_CTX\_check\_private\_key() checks the consistency of a private key with
the corresponding certificate loaded into **ctx**. If more than one
key/certificate pair (RSA/DSA) is installed, the last item installed will
be checked. If e.g. the last item was a RSA certificate or key, the RSA
key/certificate pair will be checked. SSL\_check\_private\_key() performs
the same check for **ssl**. If no key/certificate was explicitly added for
this **ssl**, the last item added into **ctx** will be checked.

# NOTES

The internal certificate store of OpenSSL can hold several private
key/certificate pairs at a time. The certificate used depends on the
cipher selected, see also [SSL\_CTX\_set\_cipher\_list(3)](http://man.he.net/man3/SSL_CTX_set_cipher_list).

When reading certificates and private keys from file, files of type
SSL\_FILETYPE\_ASN1 (also known as **DER**, binary encoding) can only contain
one certificate or private key, consequently
SSL\_CTX\_use\_certificate\_chain\_file() is only applicable to PEM formatting.
Files of type SSL\_FILETYPE\_PEM can contain more than one item.

SSL\_CTX\_use\_certificate\_chain\_file() adds the first certificate found
in the file to the certificate store. The other certificates are added
to the store of chain certificates using [SSL\_CTX\_add1\_chain\_cert(3)](http://man.he.net/man3/SSL_CTX_add1_chain_cert). Note: versions of OpenSSL before 1.0.2 only had a single
certificate chain store for all certificate types, OpenSSL 1.0.2 and later
have a separate chain store for each type. SSL\_CTX\_use\_certificate\_chain\_file()
should be used instead of the SSL\_CTX\_use\_certificate\_file() function in order
to allow the use of complete certificate chains even when no trusted CA
storage is used or when the CA issuing the certificate shall not be added to
the trusted CA storage.

If additional certificates are needed to complete the chain during the
TLS negotiation, CA certificates are additionally looked up in the
locations of trusted CA certificates, see
[SSL\_CTX\_load\_verify\_locations(3)](http://man.he.net/man3/SSL_CTX_load_verify_locations).

The private keys loaded from file can be encrypted. In order to successfully
load encrypted keys, a function returning the passphrase must have been
supplied, see
[SSL\_CTX\_set\_default\_passwd\_cb(3)](http://man.he.net/man3/SSL_CTX_set_default_passwd_cb).
(Certificate files might be encrypted as well from the technical point
of view, it however does not make sense as the data in the certificate
is considered public anyway.)

# RETURN VALUES

On success, the functions return 1.
Otherwise check out the error stack to find out the reason.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_new(3)](http://man.he.net/man3/SSL_new), [SSL\_clear(3)](http://man.he.net/man3/SSL_clear),
[SSL\_CTX\_load\_verify\_locations(3)](http://man.he.net/man3/SSL_CTX_load_verify_locations),
[SSL\_CTX\_set\_default\_passwd\_cb(3)](http://man.he.net/man3/SSL_CTX_set_default_passwd_cb),
[SSL\_CTX\_set\_cipher\_list(3)](http://man.he.net/man3/SSL_CTX_set_cipher_list),
[SSL\_CTX\_set\_client\_CA\_list(3)](http://man.he.net/man3/SSL_CTX_set_client_CA_list),
[SSL\_CTX\_set\_client\_cert\_cb(3)](http://man.he.net/man3/SSL_CTX_set_client_cert_cb),
[SSL\_CTX\_add\_extra\_chain\_cert(3)](http://man.he.net/man3/SSL_CTX_add_extra_chain_cert)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
