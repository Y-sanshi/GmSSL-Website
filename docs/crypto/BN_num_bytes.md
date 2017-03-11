# NAME

BN\_num\_bits, BN\_num\_bytes, BN\_num\_bits\_word - get BIGNUM size

# SYNOPSIS

    #include <openssl/bn.h>

    int BN_num_bytes(const BIGNUM *a);

    int BN_num_bits(const BIGNUM *a);

    int BN_num_bits_word(BN_ULONG w);

# DESCRIPTION

BN\_num\_bytes() returns the size of a **BIGNUM** in bytes.

BN\_num\_bits\_word() returns the number of significant bits in a word.
If we take 0x00000432 as an example, it returns 11, not 16, not 32.
Basically, except for a zero, it returns _floor(log2(w))+1_.

BN\_num\_bits() returns the number of significant bits in a **BIGNUM**,
following the same principle as BN\_num\_bits\_word().

BN\_num\_bytes() is a macro.

# RETURN VALUES

The size.

# NOTES

Some have tried using BN\_num\_bits() on individual numbers in RSA keys,
DH keys and DSA keys, and found that they don't always come up with
the number of bits they expected (something like 512, 1024, 2048,
...).  This is because generating a number with some specific number
of bits doesn't always set the highest bits, thereby making the number
of _significant_ bits a little lower.  If you want to know the "key
size" of such a key, either use functions like RSA\_size(), DH\_size()
and DSA\_size(), or use BN\_num\_bytes() and multiply with 8 (although
there's no real guarantee that will match the "key size", just a lot
more probability).

# SEE ALSO

[bn(3)](http://man.he.net/man3/bn), [DH\_size(3)](http://man.he.net/man3/DH_size), [DSA\_size(3)](http://man.he.net/man3/DSA_size),
[RSA\_size(3)](http://man.he.net/man3/RSA_size)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
