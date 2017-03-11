# NAME

d2i\_PrivateKey, d2i\_AutoPrivateKey, i2d\_PrivateKey,
d2i\_PrivateKey\_bio, d2i\_PrivateKey\_fp
\- decode and encode functions for reading and saving EVP\_PKEY structures

# SYNOPSIS

    #include <openssl/evp.h>

    EVP_PKEY *d2i_PrivateKey(int type, EVP_PKEY **a, const unsigned char **pp,
                             long length);
    EVP_PKEY *d2i_AutoPrivateKey(EVP_PKEY **a, const unsigned char **pp,
                                 long length);
    int i2d_PrivateKey(EVP_PKEY *a, unsigned char **pp);

    EVP_PKEY *d2i_PrivateKey_bio(BIO *bp, EVP_PKEY **a);
    EVP_PKEY *d2i_PrivateKey_fp(FILE *fp, EVP_PKEY **a)

# DESCRIPTION

d2i\_PrivateKey() decodes a private key using algorithm **type**. It attempts to
use any key specific format or PKCS#8 unencrypted PrivateKeyInfo format. The
**type** parameter should be a public key algorithm constant such as
**EVP\_PKEY\_RSA**. An error occurs if the decoded key does not match **type**.

d2i\_AutoPrivateKey() is similar to d2i\_PrivateKey() except it attempts to
automatically detect the private key format.

i2d\_PrivateKey() encodes **key**. It uses a key specific format or, if none is
defined for that key type, PKCS#8 unencrypted PrivateKeyInfo format.

These functions are similar to the d2i\_X509() functions; see [d2i\_X509(3)](http://man.he.net/man3/d2i_X509).

# NOTES

All these functions use DER format and unencrypted keys. Applications wishing
to encrypt or decrypt private keys should use other functions such as
d2i\_PKC8PrivateKey() instead.

If the **\*a** is not NULL when calling d2i\_PrivateKey() or d2i\_AutoPrivateKey()
(i.e. an existing structure is being reused) and the key format is PKCS#8
then **\*a** will be freed and replaced on a successful call.

# RETURN VALUES

d2i\_PrivateKey() and d2i\_AutoPrivateKey() return a valid **EVP\_KEY** structure
or **NULL** if an error occurs. The error code can be obtained by calling
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

i2d\_PrivateKey() returns the number of bytes successfully encoded or a
negative value if an error occurs. The error code can be obtained by calling
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# SEE ALSO

[crypto(3)](http://man.he.net/man3/crypto),
[d2i\_PKCS8PrivateKey(3)](http://man.he.net/man3/d2i_PKCS8PrivateKey)

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
