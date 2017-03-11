# NAME

EVP\_PKEY\_copy\_parameters, EVP\_PKEY\_missing\_parameters, EVP\_PKEY\_cmp\_parameters,
EVP\_PKEY\_cmp - public key parameter and comparison functions

# SYNOPSIS

    #include <openssl/evp.h>

    int EVP_PKEY_missing_parameters(const EVP_PKEY *pkey);
    int EVP_PKEY_copy_parameters(EVP_PKEY *to, const EVP_PKEY *from);

    int EVP_PKEY_cmp_parameters(const EVP_PKEY *a, const EVP_PKEY *b);
    int EVP_PKEY_cmp(const EVP_PKEY *a, const EVP_PKEY *b);

# DESCRIPTION

The function EVP\_PKEY\_missing\_parameters() returns 1 if the public key
parameters of **pkey** are missing and 0 if they are present or the algorithm
doesn't use parameters.

The function EVP\_PKEY\_copy\_parameters() copies the parameters from key
**from** to key **to**. An error is returned if the parameters are missing in
**from** or present in both **from** and **to** and mismatch. If the parameters
in **from** and **to** are both present and match this function has no effect.

The function EVP\_PKEY\_cmp\_parameters() compares the parameters of keys
**a** and **b**.

The function EVP\_PKEY\_cmp() compares the public key components and parameters
(if present) of keys **a** and **b**.

# NOTES

The main purpose of the functions EVP\_PKEY\_missing\_parameters() and
EVP\_PKEY\_copy\_parameters() is to handle public keys in certificates where the
parameters are sometimes omitted from a public key if they are inherited from
the CA that signed it.

Since OpenSSL private keys contain public key components too the function
EVP\_PKEY\_cmp() can also be used to determine if a private key matches
a public key.

# RETURN VALUES

The function EVP\_PKEY\_missing\_parameters() returns 1 if the public key
parameters of **pkey** are missing and 0 if they are present or the algorithm
doesn't use parameters.

These functions EVP\_PKEY\_copy\_parameters() returns 1 for success and 0 for
failure.

The function EVP\_PKEY\_cmp\_parameters() and EVP\_PKEY\_cmp() return 1 if the
keys match, 0 if they don't match, -1 if the key types are different and
\-2 if the operation is not supported.

# SEE ALSO

[EVP\_PKEY\_CTX\_new(3)](http://man.he.net/man3/EVP_PKEY_CTX_new),
[EVP\_PKEY\_keygen(3)](http://man.he.net/man3/EVP_PKEY_keygen)

# COPYRIGHT

Copyright 2006-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
