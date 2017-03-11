# NAME

EVP\_PKEY\_sign\_init, EVP\_PKEY\_sign - sign using a public key algorithm

# SYNOPSIS

    #include <openssl/evp.h>

    int EVP_PKEY_sign_init(EVP_PKEY_CTX *ctx);
    int EVP_PKEY_sign(EVP_PKEY_CTX *ctx,
                           unsigned char *sig, size_t *siglen,
                           const unsigned char *tbs, size_t tbslen);

# DESCRIPTION

The EVP\_PKEY\_sign\_init() function initializes a public key algorithm
context using key **pkey** for a signing operation.

The EVP\_PKEY\_sign() function performs a public key signing operation
using **ctx**. The data to be signed is specified using the **tbs** and
**tbslen** parameters. If **sig** is **NULL** then the maximum size of the output
buffer is written to the **siglen** parameter. If **sig** is not **NULL** then
before the call the **siglen** parameter should contain the length of the
**sig** buffer, if the call is successful the signature is written to
**sig** and the amount of data written to **siglen**.

# NOTES

EVP\_PKEY\_sign() does not hash the data to be signed, and therefore is
normally used to sign digests. For signing arbitrary messages, see the
[EVP\_DigestSignInit(3)](http://man.he.net/man3/EVP_DigestSignInit) and
[EVP\_SignInit(3)](http://man.he.net/man3/EVP_SignInit) signing interfaces instead.

After the call to EVP\_PKEY\_sign\_init() algorithm specific control
operations can be performed to set any appropriate parameters for the
operation (see [EVP\_PKEY\_CTX\_ctrl(3)](http://man.he.net/man3/EVP_PKEY_CTX_ctrl)).

The function EVP\_PKEY\_sign() can be called more than once on the same
context if several operations are performed using the same parameters.

# RETURN VALUES

EVP\_PKEY\_sign\_init() and EVP\_PKEY\_sign() return 1 for success and 0
or a negative value for failure. In particular a return value of -2
indicates the operation is not supported by the public key algorithm.

# EXAMPLE

Sign data using RSA with PKCS#1 padding and SHA256 digest:

    #include <openssl/evp.h>
    #include <openssl/rsa.h>

    EVP_PKEY_CTX *ctx;
    /* md is a SHA-256 digest in this example. */
    unsigned char *md, *sig;
    size_t mdlen = 32, siglen;
    EVP_PKEY *signing_key;

    /*
     * NB: assumes signing_key and md are set up before the next
     * step. signing_key must be an RSA private key and md must
     * point to the SHA-256 digest to be signed.
     */
    ctx = EVP_PKEY_CTX_new(signing_key, NULL /* no engine */);
    if (!ctx)
           /* Error occurred */
    if (EVP_PKEY_sign_init(ctx) <= 0)
           /* Error */
    if (EVP_PKEY_CTX_set_rsa_padding(ctx, RSA_PKCS1_PADDING) <= 0)
           /* Error */
    if (EVP_PKEY_CTX_set_signature_md(ctx, EVP_sha256()) <= 0)
           /* Error */

    /* Determine buffer length */
    if (EVP_PKEY_sign(ctx, NULL, &siglen, md, mdlen) <= 0)
           /* Error */

    sig = OPENSSL_malloc(siglen);

    if (!sig)
           /* malloc failure */

    if (EVP_PKEY_sign(ctx, sig, &siglen, md, mdlen) <= 0)
           /* Error */

    /* Signature is siglen bytes written to buffer sig */

# SEE ALSO

[EVP\_PKEY\_CTX\_new(3)](http://man.he.net/man3/EVP_PKEY_CTX_new),
[EVP\_PKEY\_CTX\_ctrl(3)](http://man.he.net/man3/EVP_PKEY_CTX_ctrl),
[EVP\_PKEY\_encrypt(3)](http://man.he.net/man3/EVP_PKEY_encrypt),
[EVP\_PKEY\_decrypt(3)](http://man.he.net/man3/EVP_PKEY_decrypt),
[EVP\_PKEY\_verify(3)](http://man.he.net/man3/EVP_PKEY_verify),
[EVP\_PKEY\_verify\_recover(3)](http://man.he.net/man3/EVP_PKEY_verify_recover),
[EVP\_PKEY\_derive(3)](http://man.he.net/man3/EVP_PKEY_derive)

# HISTORY

These functions were first added to OpenSSL 1.0.0.

# COPYRIGHT

Copyright 2006-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
