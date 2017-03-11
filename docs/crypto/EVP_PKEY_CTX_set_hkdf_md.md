# NAME

EVP\_PKEY\_CTX\_set\_hkdf\_md, EVP\_PKEY\_CTX\_set1\_hkdf\_salt,
EVP\_PKEY\_CTX\_set1\_hkdf\_key, EVP\_PKEY\_CTX\_add1\_hkdf\_info -
HMAC-based Extract-and-Expand key derivation algorithm

# SYNOPSIS

    #include <openssl/kdf.h>

    int EVP_PKEY_CTX_set_hkdf_md(EVP_PKEY_CTX *pctx, const EVP_MD *md);

    int EVP_PKEY_CTX_set1_hkdf_salt(EVP_PKEY_CTX *pctx, unsigned char *salt,
                                    int saltlen);

    int EVP_PKEY_CTX_set1_hkdf_key(EVP_PKEY_CTX *pctx, unsigned char *key,
                                   int keylen);

    int EVP_PKEY_CTX_add1_hkdf_info(EVP_PKEY_CTX *pctx, unsigned char *info,
                                    int infolen);

# DESCRIPTION

The EVP\_PKEY\_HKDF algorithm implements the HKDF key derivation function.
HKDF follows the "extract-then-expand" paradigm, where the KDF logically
consists of two modules. The first stage takes the input keying material
and "extracts" from it a fixed-length pseudorandom key K. The second stage
"expands" the key K into several additional pseudorandom keys (the output
of the KDF).

EVP\_PKEY\_set\_hkdf\_md() sets the message digest associated with the HKDF.

EVP\_PKEY\_CTX\_set1\_hkdf\_salt() sets the salt to **saltlen** bytes of the
buffer **salt**. Any existing value is replaced.

EVP\_PKEY\_CTX\_set\_hkdf\_key() sets the key to **keylen** bytes of the buffer
**key**. Any existing value is replaced.

EVP\_PKEY\_CTX\_add1\_hkdf\_info() sets the info value to **infolen** bytes of the
buffer **info**. If a value is already set, it is appended to the existing
value.

# STRING CTRLS

HKDF also supports string based control operations via
[EVP\_PKEY\_CTX\_ctrl\_str(3)](http://man.he.net/man3/EVP_PKEY_CTX_ctrl_str).
The **type** parameter "md" uses the supplied **value** as the name of the digest
algorithm to use.
The **type** parameters "salt", "key" and "info" use the supplied **value**
parameter as a **seed**, **key** or **info** value.
The names "hexsalt", "hexkey" and "hexinfo" are similar except they take a hex
string which is converted to binary.

# NOTES

All these functions are implemented as macros.

A context for HKDF can be obtained by calling:

    EVP_PKEY_CTX *pctx = EVP_PKEY_new_id(EVP_PKEY_HKDF, NULL);

The digest, key, salt and info values must be set before a key is derived or
an error occurs.

The total length of the info buffer cannot exceed 1024 bytes in length: this
should be more than enough for any normal use of HKDF.

The output length of the KDF is specified via the length parameter to the
[EVP\_PKEY\_derive(3)](http://man.he.net/man3/EVP_PKEY_derive) function.
Since the HKDF output length is variable, passing a **NULL** buffer as a means
to obtain the requisite length is not meaningful with HKDF.
Instead, the caller must allocate a buffer of the desired length, and pass that
buffer to [EVP\_PKEY\_derive(3)](http://man.he.net/man3/EVP_PKEY_derive) along with (a pointer initialized to) the
desired length.

Optimised versions of HKDF can be implemented in an ENGINE.

# RETURN VALUES

All these functions return 1 for success and 0 or a negative value for failure.
In particular a return value of -2 indicates the operation is not supported by
the public key algorithm.

# EXAMPLE

This example derives 10 bytes using SHA-256 with the secret key "secret",
salt value "salt" and info value "label":

    EVP_PKEY_CTX *pctx;
    unsigned char out[10];
    size_t outlen = sizeof(out);
    pctx = EVP_PKEY_CTX_new_id(EVP_PKEY_HKDF, NULL);

    if (EVP_PKEY_derive_init(pctx) <= 0)
       /* Error */
    if (EVP_PKEY_CTX_set_hkdf_md(pctx, EVP_sha256()) <= 0)
       /* Error */
    if (EVP_PKEY_CTX_set1_salt(pctx, "salt", 4) <= 0)
       /* Error */
    if (EVP_PKEY_CTX_set1_key(pctx, "secret", 6) <= 0)
       /* Error */
    if (EVP_PKEY_CTX_add1_hkdf_info(pctx, "label", 6) <= 0)
       /* Error */
    if (EVP_PKEY_derive(pctx, out, &outlen) <= 0)
       /* Error */

# CONFORMING TO

RFC 5869

# SEE ALSO

[EVP\_PKEY\_CTX\_new(3)](http://man.he.net/man3/EVP_PKEY_CTX_new),
[EVP\_PKEY\_CTX\_ctrl\_str(3)](http://man.he.net/man3/EVP_PKEY_CTX_ctrl_str),
[EVP\_PKEY\_derive(3)](http://man.he.net/man3/EVP_PKEY_derive)

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
