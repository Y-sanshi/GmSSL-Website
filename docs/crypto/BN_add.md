# NAME

BN\_add, BN\_sub, BN\_mul, BN\_sqr, BN\_div, BN\_mod, BN\_nnmod, BN\_mod\_add,
BN\_mod\_sub, BN\_mod\_mul, BN\_mod\_sqr, BN\_exp, BN\_mod\_exp, BN\_gcd -
arithmetic operations on BIGNUMs

# SYNOPSIS

    #include <openssl/bn.h>

    int BN_add(BIGNUM *r, const BIGNUM *a, const BIGNUM *b);

    int BN_sub(BIGNUM *r, const BIGNUM *a, const BIGNUM *b);

    int BN_mul(BIGNUM *r, BIGNUM *a, BIGNUM *b, BN_CTX *ctx);

    int BN_sqr(BIGNUM *r, BIGNUM *a, BN_CTX *ctx);

    int BN_div(BIGNUM *dv, BIGNUM *rem, const BIGNUM *a, const BIGNUM *d,
            BN_CTX *ctx);

    int BN_mod(BIGNUM *rem, const BIGNUM *a, const BIGNUM *m, BN_CTX *ctx);

    int BN_nnmod(BIGNUM *r, const BIGNUM *a, const BIGNUM *m, BN_CTX *ctx);

    int BN_mod_add(BIGNUM *r, BIGNUM *a, BIGNUM *b, const BIGNUM *m,
            BN_CTX *ctx);

    int BN_mod_sub(BIGNUM *r, BIGNUM *a, BIGNUM *b, const BIGNUM *m,
            BN_CTX *ctx);

    int BN_mod_mul(BIGNUM *r, BIGNUM *a, BIGNUM *b, const BIGNUM *m,
            BN_CTX *ctx);

    int BN_mod_sqr(BIGNUM *r, BIGNUM *a, const BIGNUM *m, BN_CTX *ctx);

    int BN_exp(BIGNUM *r, BIGNUM *a, BIGNUM *p, BN_CTX *ctx);

    int BN_mod_exp(BIGNUM *r, BIGNUM *a, const BIGNUM *p,
            const BIGNUM *m, BN_CTX *ctx);

    int BN_gcd(BIGNUM *r, BIGNUM *a, BIGNUM *b, BN_CTX *ctx);

# DESCRIPTION

BN\_add() adds _a_ and _b_ and places the result in _r_ (`r=a+b`).
_r_ may be the same **BIGNUM** as _a_ or _b_.

BN\_sub() subtracts _b_ from _a_ and places the result in _r_ (`r=a-b`).
_r_ may be the same **BIGNUM** as _a_ or _b_.

BN\_mul() multiplies _a_ and _b_ and places the result in _r_ (`r=a*b`).
_r_ may be the same **BIGNUM** as _a_ or _b_.
For multiplication by powers of 2, use [BN\_lshift(3)](http://man.he.net/man3/BN_lshift).

BN\_sqr() takes the square of _a_ and places the result in _r_
(`r=a^2`). _r_ and _a_ may be the same **BIGNUM**.
This function is faster than BN\_mul(r,a,a).

BN\_div() divides _a_ by _d_ and places the result in _dv_ and the
remainder in _rem_ (`dv=a/d, rem=a%d`). Either of _dv_ and _rem_ may
be **NULL**, in which case the respective value is not returned.
The result is rounded towards zero; thus if _a_ is negative, the
remainder will be zero or negative.
For division by powers of 2, use BN\_rshift(3).

BN\_mod() corresponds to BN\_div() with _dv_ set to **NULL**.

BN\_nnmod() reduces _a_ modulo _m_ and places the non-negative
remainder in _r_.

BN\_mod\_add() adds _a_ to _b_ modulo _m_ and places the non-negative
result in _r_.

BN\_mod\_sub() subtracts _b_ from _a_ modulo _m_ and places the
non-negative result in _r_.

BN\_mod\_mul() multiplies _a_ by _b_ and finds the non-negative
remainder respective to modulus _m_ (`r=(a*b) mod m`). _r_ may be
the same **BIGNUM** as _a_ or _b_. For more efficient algorithms for
repeated computations using the same modulus, see
[BN\_mod\_mul\_montgomery(3)](http://man.he.net/man3/BN_mod_mul_montgomery) and
[BN\_mod\_mul\_reciprocal(3)](http://man.he.net/man3/BN_mod_mul_reciprocal).

BN\_mod\_sqr() takes the square of _a_ modulo **m** and places the
result in _r_.

BN\_exp() raises _a_ to the _p_-th power and places the result in _r_
(`r=a^p`). This function is faster than repeated applications of
BN\_mul().

BN\_mod\_exp() computes _a_ to the _p_-th power modulo _m_ (`r=a^p %
m`). This function uses less time and space than BN\_exp().

BN\_gcd() computes the greatest common divisor of _a_ and _b_ and
places the result in _r_. _r_ may be the same **BIGNUM** as _a_ or
_b_.

For all functions, _ctx_ is a previously allocated **BN\_CTX** used for
temporary variables; see [BN\_CTX\_new(3)](http://man.he.net/man3/BN_CTX_new).

Unless noted otherwise, the result **BIGNUM** must be different from
the arguments.

# RETURN VALUES

For all functions, 1 is returned for success, 0 on error. The return
value should always be checked (e.g., `if (!BN_add(r,a,b)) goto err;`).
The error codes can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# SEE ALSO

[bn(3)](http://man.he.net/man3/bn), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [BN\_CTX\_new(3)](http://man.he.net/man3/BN_CTX_new),
[BN\_add\_word(3)](http://man.he.net/man3/BN_add_word), [BN\_set\_bit(3)](http://man.he.net/man3/BN_set_bit)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
