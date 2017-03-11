# NAME

BN\_set\_bit, BN\_clear\_bit, BN\_is\_bit\_set, BN\_mask\_bits, BN\_lshift,
BN\_lshift1, BN\_rshift, BN\_rshift1 - bit operations on BIGNUMs

# SYNOPSIS

    #include <openssl/bn.h>

    int BN_set_bit(BIGNUM *a, int n);
    int BN_clear_bit(BIGNUM *a, int n);

    int BN_is_bit_set(const BIGNUM *a, int n);

    int BN_mask_bits(BIGNUM *a, int n);

    int BN_lshift(BIGNUM *r, const BIGNUM *a, int n);
    int BN_lshift1(BIGNUM *r, BIGNUM *a);

    int BN_rshift(BIGNUM *r, BIGNUM *a, int n);
    int BN_rshift1(BIGNUM *r, BIGNUM *a);

# DESCRIPTION

BN\_set\_bit() sets bit **n** in **a** to 1 (`a|=(1<<n)`). The
number is expanded if necessary.

BN\_clear\_bit() sets bit **n** in **a** to 0 (`a&=~(1<<n)`). An
error occurs if **a** is shorter than **n** bits.

BN\_is\_bit\_set() tests if bit **n** in **a** is set.

BN\_mask\_bits() truncates **a** to an **n** bit number
(`a&=~((~0)>>n)`).  An error occurs if **a** already is
shorter than **n** bits.

BN\_lshift() shifts **a** left by **n** bits and places the result in
**r** (`r=a*2^n`). Note that **n** must be non-negative. BN\_lshift1() shifts
**a** left by one and places the result in **r** (`r=2*a`).

BN\_rshift() shifts **a** right by **n** bits and places the result in
**r** (`r=a/2^n`). Note that **n** must be non-negative. BN\_rshift1() shifts
**a** right by one and places the result in **r** (`r=a/2`).

For the shift functions, **r** and **a** may be the same variable.

# RETURN VALUES

BN\_is\_bit\_set() returns 1 if the bit is set, 0 otherwise.

All other functions return 1 for success, 0 on error. The error codes
can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# SEE ALSO

[bn(3)](http://man.he.net/man3/bn), [BN\_num\_bytes(3)](http://man.he.net/man3/BN_num_bytes), [BN\_add(3)](http://man.he.net/man3/BN_add)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
