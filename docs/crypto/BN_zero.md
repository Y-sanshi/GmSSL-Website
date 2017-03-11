# NAME

BN\_zero, BN\_one, BN\_value\_one, BN\_set\_word, BN\_get\_word - BIGNUM assignment
operations

# SYNOPSIS

    #include <openssl/bn.h>

    void BN_zero(BIGNUM *a);
    int BN_one(BIGNUM *a);

    const BIGNUM *BN_value_one(void);

    int BN_set_word(BIGNUM *a, unsigned long w);
    unsigned long BN_get_word(BIGNUM *a);

Deprecated:

    #if OPENSSL_API_COMPAT < 0x00908000L
    int BN_zero(BIGNUM *a);
    #endif

# DESCRIPTION

BN\_zero(), BN\_one() and BN\_set\_word() set **a** to the values 0, 1 and
**w** respectively.  BN\_zero() and BN\_one() are macros.

BN\_value\_one() returns a **BIGNUM** constant of value 1. This constant
is useful for use in comparisons and assignment.

BN\_get\_word() returns **a**, if it can be represented as an unsigned
long.

# RETURN VALUES

BN\_get\_word() returns the value **a**, and 0xffffffffL if **a** cannot
be represented as an unsigned long.

BN\_one(), BN\_set\_word() and the deprecated version of BN\_zero()
return 1 on success, 0 otherwise.
BN\_value\_one() returns the constant.
The preferred version of BN\_zero() never fails and returns no value.

# BUGS

Someone might change the constant.

If a **BIGNUM** is equal to 0xffffffffL it can be represented as an
unsigned long but this value is also returned on error.

# SEE ALSO

[bn(3)](http://man.he.net/man3/bn), [BN\_bn2bin(3)](http://man.he.net/man3/BN_bn2bin)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
