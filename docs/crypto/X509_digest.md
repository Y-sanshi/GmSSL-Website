# NAME

X509\_digest, X509\_CRL\_digest,
X509\_pubkey\_digest,
X509\_NAME\_digest,
X509\_REQ\_digest
PKCS7\_ISSUER\_AND\_SERIAL\_digest,
\- get digest of various objects

# SYNOPSIS

    #include <openssl/x509.h>

    int X509_digest(const X509 *data, const EVP_MD *type, unsigned char *md,
                    unsigned int *len);

    int X509_CRL_digest(const X509_CRL *data, const EVP_MD *type, unsigned char *md,
                    unsigned int *len);

    int X509_pubkey_digest(const X509 *data, const EVP_MD *type,
                           unsigned char *md, unsigned int *len);

    int X509_REQ_digest(const X509_REQ *data, const EVP_MD *type,
                        unsigned char *md, unsigned int *len);

    int X509_NAME_digest(const X509_NAME *data, const EVP_MD *type,
                         unsigned char *md, unsigned int *len);

    int PKCS7_ISSUER_AND_SERIAL_digest(PKCS7_ISSUER_AND_SERIAL *data,
                                       const EVP_MD *type, unsigned char *md,
                                       unsigned int *len);

# DESCRIPTION

X509\_pubkey\_digest() returns a digest of the DER representation of the public
key in the specified X509 **data** object.
All other functions described here return a digest of the DER representation
of their entire **data** objects.

The **type** parameter specifies the digest to
be used, such as EVP\_sha1(). The **md** is a pointer to the buffer where the
digest will be copied and is assumed to be large enough; the constant
**EVP\_MAX\_MD\_SIZE** is suggested. The **len** parameter, if not NULL, points
to a place where the digest size will be stored.

# RETURN VALUES

All functions described here return 1 for success and 0 for failure.

# SEE ALSO

[EVP\_SHA1(3)](http://man.he.net/man3/EVP_SHA1)

# COPYRIGHT

Copyright 2017 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
