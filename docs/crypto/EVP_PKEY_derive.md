# NAME

EVP\_PKEY\_derive\_init, EVP\_PKEY\_derive\_set\_peer, EVP\_PKEY\_derive - derive public key algorithm shared secret

# SYNOPSIS

    #include <openssl/evp.h>

    int EVP_PKEY_derive_init(EVP_PKEY_CTX *ctx);
    int EVP_PKEY_derive_set_peer(EVP_PKEY_CTX *ctx, EVP_PKEY *peer);
    int EVP_PKEY_derive(EVP_PKEY_CTX *ctx, unsigned char *key, size_t *keylen);

# DESCRIPTION

The EVP\_PKEY\_derive\_init() function initializes a public key algorithm
context using key **pkey** for shared secret derivation.

The EVP\_PKEY\_derive\_set\_peer() function sets the peer key: this will normally
be a public key.

The EVP\_PKEY\_derive() derives a shared secret using **ctx**.
If **key** is **NULL** then the maximum size of the output buffer is written to
the **keylen** parameter. If **key** is not **NULL** then before the call the
**keylen** parameter should contain the length of the **key** buffer, if the call
is successful the shared secret is written to **key** and the amount of data
written to **keylen**.

# NOTES

After the call to EVP\_PKEY\_derive\_init() algorithm specific control
operations can be performed to set any appropriate parameters for the
operation.

The function EVP\_PKEY\_derive() can be called more than once on the same
context if several operations are performed using the same parameters.

# RETURN VALUES

EVP\_PKEY\_derive\_init() and EVP\_PKEY\_derive() return 1 for success and 0
or a negative value for failure. In particular a return value of -2
indicates the operation is not supported by the public key algorithm.

# EXAMPLE

Derive shared secret (for example DH or EC keys):

    #include <openssl/evp.h>
    #include <openssl/rsa.h>

    EVP_PKEY_CTX *ctx;
    unsigned char *skey;
    size_t skeylen;
    EVP_PKEY *pkey, *peerkey;
    /* NB: assumes pkey, peerkey have been already set up */

    ctx = EVP_PKEY_CTX_new(pkey);
    if (!ctx)
           /* Error occurred */
    if (EVP_PKEY_derive_init(ctx) <= 0)
           /* Error */
    if (EVP_PKEY_derive_set_peer(ctx, peerkey) <= 0)
           /* Error */

    /* Determine buffer length */
    if (EVP_PKEY_derive(ctx, NULL, &skeylen) <= 0)
           /* Error */

    skey = OPENSSL_malloc(skeylen);

    if (!skey)
           /* malloc failure */

    if (EVP_PKEY_derive(ctx, skey, &skeylen) <= 0)
           /* Error */

    /* Shared secret is skey bytes written to buffer skey */

# SEE ALSO

[EVP\_PKEY\_CTX\_new(3)](http://man.he.net/man3/EVP_PKEY_CTX_new),
[EVP\_PKEY\_encrypt(3)](http://man.he.net/man3/EVP_PKEY_encrypt),
[EVP\_PKEY\_decrypt(3)](http://man.he.net/man3/EVP_PKEY_decrypt),
[EVP\_PKEY\_sign(3)](http://man.he.net/man3/EVP_PKEY_sign),
[EVP\_PKEY\_verify(3)](http://man.he.net/man3/EVP_PKEY_verify),
[EVP\_PKEY\_verify\_recover(3)](http://man.he.net/man3/EVP_PKEY_verify_recover),

# HISTORY

These functions were first added to OpenSSL 1.0.0.

# COPYRIGHT

Copyright 2006-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
