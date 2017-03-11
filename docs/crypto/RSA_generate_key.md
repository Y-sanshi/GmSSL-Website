# NAME

RSA\_generate\_key\_ex, RSA\_generate\_key - generate RSA key pair

# SYNOPSIS

    #include <openssl/rsa.h>

    int RSA_generate_key_ex(RSA *rsa, int bits, BIGNUM *e, BN_GENCB *cb);

Deprecated:

    #if OPENSSL_API_COMPAT < 0x00908000L
    RSA *RSA_generate_key(int num, unsigned long e,
       void (*callback)(int, int, void *), void *cb_arg);
    #endif

# DESCRIPTION

RSA\_generate\_key\_ex() generates a key pair and stores it in the **RSA**
structure provided in **rsa**. The pseudo-random number generator must
be seeded prior to calling RSA\_generate\_key\_ex().

The modulus size will be of length **bits**, and the public exponent will be
**e**. Key sizes with **num** < 1024 should be considered insecure.
The exponent is an odd number, typically 3, 17 or 65537.

A callback function may be used to provide feedback about the
progress of the key generation. If **cb** is not **NULL**, it
will be called as follows using the BN\_GENCB\_call() function
described on the [BN\_generate\_prime(3)](http://man.he.net/man3/BN_generate_prime) page.

- While a random prime number is generated, it is called as
described in [BN\_generate\_prime(3)](http://man.he.net/man3/BN_generate_prime).
- When the n-th randomly generated prime is rejected as not
suitable for the key, **BN\_GENCB\_call(cb, 2, n)** is called.
- When a random p has been found with p-1 relatively prime to **e**,
it is called as **BN\_GENCB\_call(cb, 3, 0)**.

The process is then repeated for prime q with **BN\_GENCB\_call(cb, 3, 1)**.

RSA\_generate\_key is deprecated (new applications should use
RSA\_generate\_key\_ex instead). RSA\_generate\_key works in the same way as
RSA\_generate\_key\_ex except it uses "old style" call backs. See
[BN\_generate\_prime(3)](http://man.he.net/man3/BN_generate_prime) for further details.

# RETURN VALUE

If key generation fails, RSA\_generate\_key() returns **NULL**.

The error codes can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# BUGS

**BN\_GENCB\_call(cb, 2, x)** is used with two different meanings.

RSA\_generate\_key() goes into an infinite loop for illegal input values.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [rand(3)](http://man.he.net/man3/rand),
[RSA\_generate\_key(3)](http://man.he.net/man3/RSA_generate_key), [BN\_generate\_prime(3)](http://man.he.net/man3/BN_generate_prime)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
