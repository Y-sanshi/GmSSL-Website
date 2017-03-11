# NAME

BN\_add\_word, BN\_sub\_word, BN\_mul\_word, BN\_div\_word, BN\_mod\_word - arithmetic
functions on BIGNUMs with integers

# SYNOPSIS

    #include <openssl/bn.h>

    int BN_add_word(BIGNUM *a, BN_ULONG w);

    int BN_sub_word(BIGNUM *a, BN_ULONG w);

    int BN_mul_word(BIGNUM *a, BN_ULONG w);

    BN_ULONG BN_div_word(BIGNUM *a, BN_ULONG w);

    BN_ULONG BN_mod_word(const BIGNUM *a, BN_ULONG w);

# DESCRIPTION

These functions perform arithmetic operations on BIGNUMs with unsigned
integers. They are much more efficient than the normal BIGNUM
arithmetic operations.

BN\_add\_word() adds **w** to **a** (`a+=w`).

BN\_sub\_word() subtracts **w** from **a** (`a-=w`).

BN\_mul\_word() multiplies **a** and **w** (`a*=w`).

BN\_div\_word() divides **a** by **w** (`a/=w`) and returns the remainder.

BN\_mod\_word() returns the remainder of **a** divided by **w** (`a%w`).

For BN\_div\_word() and BN\_mod\_word(), **w** must not be 0.

# RETURN VALUES

BN\_add\_word(), BN\_sub\_word() and BN\_mul\_word() return 1 for success, 0
on error. The error codes can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

BN\_mod\_word() and BN\_div\_word() return **a**%**w** on success and
**(BN\_ULONG)-1** if an error occurred.

# SEE ALSO

[bn(3)](http://man.he.net/man3/bn), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [BN\_add(3)](http://man.he.net/man3/BN_add)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
