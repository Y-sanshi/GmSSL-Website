# NAME

d2i\_PKCS8PrivateKey\_bio, d2i\_PKCS8PrivateKey\_fp,
i2d\_PKCS8PrivateKey\_bio, i2d\_PKCS8PrivateKey\_fp,
i2d\_PKCS8PrivateKey\_nid\_bio, i2d\_PKCS8PrivateKey\_nid\_fp - PKCS#8 format private key functions

# SYNOPSIS

    #include <openssl/evp.h>

    EVP_PKEY *d2i_PKCS8PrivateKey_bio(BIO *bp, EVP_PKEY **x, pem_password_cb *cb, void *u);
    EVP_PKEY *d2i_PKCS8PrivateKey_fp(FILE *fp, EVP_PKEY **x, pem_password_cb *cb, void *u);

    int i2d_PKCS8PrivateKey_bio(BIO *bp, EVP_PKEY *x, const EVP_CIPHER *enc,
                                     char *kstr, int klen,
                                     pem_password_cb *cb, void *u);

    int i2d_PKCS8PrivateKey_fp(FILE *fp, EVP_PKEY *x, const EVP_CIPHER *enc,
                                     char *kstr, int klen,
                                     pem_password_cb *cb, void *u);

    int i2d_PKCS8PrivateKey_nid_bio(BIO *bp, EVP_PKEY *x, int nid,
                                     char *kstr, int klen,
                                     pem_password_cb *cb, void *u);

    int i2d_PKCS8PrivateKey_nid_fp(FILE *fp, EVP_PKEY *x, int nid,
                                     char *kstr, int klen,
                                     pem_password_cb *cb, void *u);

# DESCRIPTION

The PKCS#8 functions encode and decode private keys in PKCS#8 format using both
PKCS#5 v1.5 and PKCS#5 v2.0 password based encryption algorithms.

Other than the use of DER as opposed to PEM these functions are identical to the
corresponding **PEM** function as described in [PEM\_read\_PrivateKey(3)](http://man.he.net/man3/PEM_read_PrivateKey).

# NOTES

These functions are currently the only way to store encrypted private keys using DER format.

Currently all the functions use BIOs or FILE pointers, there are no functions which
work directly on memory: this can be readily worked around by converting the buffers
to memory BIOs, see [BIO\_s\_mem(3)](http://man.he.net/man3/BIO_s_mem) for details.

# SEE ALSO

[PEM\_read\_PrivateKey(3)](http://man.he.net/man3/PEM_read_PrivateKey)

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
