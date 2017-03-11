# NAME

BN\_CTX\_new, BN\_CTX\_secure\_new, BN\_CTX\_free - allocate and free BN\_CTX structures

# SYNOPSIS

    #include <openssl/bn.h>

    BN_CTX *BN_CTX_new(void);

    BN_CTX *BN_CTX_secure_new(void);

    void BN_CTX_free(BN_CTX *c);

# DESCRIPTION

A **BN\_CTX** is a structure that holds **BIGNUM** temporary variables used by
library functions. Since dynamic memory allocation to create **BIGNUM**s
is rather expensive when used in conjunction with repeated subroutine
calls, the **BN\_CTX** structure is used.

BN\_CTX\_new() allocates and initializes a **BN\_CTX** structure.
BN\_CTX\_secure\_new() allocates and initializes a **BN\_CTX** structure
but uses the secure heap (see [CRYPTO\_secure\_malloc(3)](http://man.he.net/man3/CRYPTO_secure_malloc)) to hold the
**BIGNUM**s.

BN\_CTX\_free() frees the components of the **BN\_CTX**, and if it was
created by BN\_CTX\_new(), also the structure itself.
If [BN\_CTX\_start(3)](http://man.he.net/man3/BN_CTX_start) has been used on the **BN\_CTX**,
[BN\_CTX\_end(3)](http://man.he.net/man3/BN_CTX_end) must be called before the **BN\_CTX**
may be freed by BN\_CTX\_free().
If **c** is NULL, nothing is done.

# RETURN VALUES

BN\_CTX\_new() and BN\_CTX\_secure\_new() return a pointer to the **BN\_CTX**.
If the allocation fails,
they return **NULL** and sets an error code that can be obtained by
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

BN\_CTX\_free() has no return values.

# REMOVED FUNCTIONALITY

    void BN_CTX_init(BN_CTX *c);

BN\_CTX\_init() is no longer available as of OpenSSL 1.1.0. Applications should
replace use of BN\_CTX\_init with BN\_CTX\_new instead:

    BN_CTX *ctx;
    ctx = BN_CTX_new();
    if(!ctx) /* Handle error */
    ...
    BN_CTX_free(ctx);

# SEE ALSO

[bn(3)](http://man.he.net/man3/bn), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [BN\_add(3)](http://man.he.net/man3/BN_add),
[BN\_CTX\_start(3)](http://man.he.net/man3/BN_CTX_start)

# HISTORY

BN\_CTX\_init() was removed in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
