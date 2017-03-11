# NAME

BN\_mod\_mul\_montgomery, BN\_MONT\_CTX\_new,
BN\_MONT\_CTX\_free, BN\_MONT\_CTX\_set, BN\_MONT\_CTX\_copy,
BN\_from\_montgomery, BN\_to\_montgomery - Montgomery multiplication

# SYNOPSIS

    #include <openssl/bn.h>

    BN_MONT_CTX *BN_MONT_CTX_new(void);
    void BN_MONT_CTX_free(BN_MONT_CTX *mont);

    int BN_MONT_CTX_set(BN_MONT_CTX *mont, const BIGNUM *m, BN_CTX *ctx);
    BN_MONT_CTX *BN_MONT_CTX_copy(BN_MONT_CTX *to, BN_MONT_CTX *from);

    int BN_mod_mul_montgomery(BIGNUM *r, BIGNUM *a, BIGNUM *b,
            BN_MONT_CTX *mont, BN_CTX *ctx);

    int BN_from_montgomery(BIGNUM *r, BIGNUM *a, BN_MONT_CTX *mont,
            BN_CTX *ctx);

    int BN_to_montgomery(BIGNUM *r, BIGNUM *a, BN_MONT_CTX *mont,
            BN_CTX *ctx);

# DESCRIPTION

These functions implement Montgomery multiplication. They are used
automatically when [BN\_mod\_exp(3)](http://man.he.net/man3/BN_mod_exp) is called with suitable input,
but they may be useful when several operations are to be performed
using the same modulus.

BN\_MONT\_CTX\_new() allocates and initializes a **BN\_MONT\_CTX** structure.

BN\_MONT\_CTX\_set() sets up the _mont_ structure from the modulus _m_
by precomputing its inverse and a value R.

BN\_MONT\_CTX\_copy() copies the **BN\_MONT\_CTX** _from_ to _to_.

BN\_MONT\_CTX\_free() frees the components of the **BN\_MONT\_CTX**, and, if
it was created by BN\_MONT\_CTX\_new(), also the structure itself.
If **mont** is NULL, nothing is done.

BN\_mod\_mul\_montgomery() computes Mont(_a_,_b_):=_a_\*_b_\*R^-1 and places
the result in _r_.

BN\_from\_montgomery() performs the Montgomery reduction _r_ = _a_\*R^-1.

BN\_to\_montgomery() computes Mont(_a_,R^2), i.e. _a_\*R.
Note that _a_ must be non-negative and smaller than the modulus.

For all functions, _ctx_ is a previously allocated **BN\_CTX** used for
temporary variables.

# RETURN VALUES

BN\_MONT\_CTX\_new() returns the newly allocated **BN\_MONT\_CTX**, and NULL
on error.

BN\_MONT\_CTX\_free() has no return value.

For the other functions, 1 is returned for success, 0 on error.
The error codes can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# WARNING

The inputs must be reduced modulo **m**, otherwise the result will be
outside the expected range.

# SEE ALSO

[bn(3)](http://man.he.net/man3/bn), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [BN\_add(3)](http://man.he.net/man3/BN_add),
[BN\_CTX\_new(3)](http://man.he.net/man3/BN_CTX_new)

# HISTORY

BN\_MONT\_CTX\_init() was removed in OpenSSL 1.1.0

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
