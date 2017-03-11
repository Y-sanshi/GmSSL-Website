# NAME

EVP\_PKEY\_CTX\_new, EVP\_PKEY\_CTX\_new\_id, EVP\_PKEY\_CTX\_dup, EVP\_PKEY\_CTX\_free - public key algorithm context functions

# SYNOPSIS

    #include <openssl/evp.h>

    EVP_PKEY_CTX *EVP_PKEY_CTX_new(EVP_PKEY *pkey, ENGINE *e);
    EVP_PKEY_CTX *EVP_PKEY_CTX_new_id(int id, ENGINE *e);
    EVP_PKEY_CTX *EVP_PKEY_CTX_dup(EVP_PKEY_CTX *ctx);
    void EVP_PKEY_CTX_free(EVP_PKEY_CTX *ctx);

# DESCRIPTION

The EVP\_PKEY\_CTX\_new() function allocates public key algorithm context using
the algorithm specified in **pkey** and ENGINE **e**.

The EVP\_PKEY\_CTX\_new\_id() function allocates public key algorithm context
using the algorithm specified by **id** and ENGINE **e**. It is normally used
when no **EVP\_PKEY** structure is associated with the operations, for example
during parameter generation of key generation for some algorithms.

EVP\_PKEY\_CTX\_dup() duplicates the context **ctx**.

EVP\_PKEY\_CTX\_free() frees up the context **ctx**.
If **ctx** is NULL, nothing is done.

# NOTES

The **EVP\_PKEY\_CTX** structure is an opaque public key algorithm context used
by the OpenSSL high level public key API. Contexts **MUST NOT** be shared between
threads: that is it is not permissible to use the same context simultaneously
in two threads.

# RETURN VALUES

EVP\_PKEY\_CTX\_new(), EVP\_PKEY\_CTX\_new\_id(), EVP\_PKEY\_CTX\_dup() returns either
the newly allocated **EVP\_PKEY\_CTX** structure of **NULL** if an error occurred.

EVP\_PKEY\_CTX\_free() does not return a value.

# SEE ALSO

[EVP\_PKEY\_new(3)](http://man.he.net/man3/EVP_PKEY_new)

# HISTORY

These functions were first added to OpenSSL 1.0.0.

# COPYRIGHT

Copyright 2006-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
