# NAME

BN\_cmp, BN\_ucmp, BN\_is\_zero, BN\_is\_one, BN\_is\_word, BN\_is\_odd - BIGNUM comparison and test functions

# SYNOPSIS

    #include <openssl/bn.h>

    int BN_cmp(BIGNUM *a, BIGNUM *b);
    int BN_ucmp(BIGNUM *a, BIGNUM *b);

    int BN_is_zero(BIGNUM *a);
    int BN_is_one(BIGNUM *a);
    int BN_is_word(BIGNUM *a, BN_ULONG w);
    int BN_is_odd(BIGNUM *a);

# DESCRIPTION

BN\_cmp() compares the numbers **a** and **b**. BN\_ucmp() compares their
absolute values.

BN\_is\_zero(), BN\_is\_one() and BN\_is\_word() test if **a** equals 0, 1,
or **w** respectively. BN\_is\_odd() tests if a is odd.

BN\_is\_zero(), BN\_is\_one(), BN\_is\_word() and BN\_is\_odd() are macros.

# RETURN VALUES

BN\_cmp() returns -1 if **a** < **b**, 0 if **a** == **b** and 1 if
**a** > **b**. BN\_ucmp() is the same using the absolute values
of **a** and **b**.

BN\_is\_zero(), BN\_is\_one() BN\_is\_word() and BN\_is\_odd() return 1 if
the condition is true, 0 otherwise.

# SEE ALSO

[bn(3)](http://man.he.net/man3/bn)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
