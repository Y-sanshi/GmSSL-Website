# NAME

DSA\_generate\_parameters\_ex, DSA\_generate\_parameters - generate DSA parameters

# SYNOPSIS

    #include <openssl/dsa.h>

    int DSA_generate_parameters_ex(DSA *dsa, int bits,
                   const unsigned char *seed, int seed_len,
                   int *counter_ret, unsigned long *h_ret, BN_GENCB *cb);

Deprecated:

    #if OPENSSL_API_COMPAT < 0x00908000L
    DSA *DSA_generate_parameters(int bits, unsigned char *seed,
                   int seed_len, int *counter_ret, unsigned long *h_ret,
                   void (*callback)(int, int, void *), void *cb_arg);
    #endif

# DESCRIPTION

DSA\_generate\_parameters\_ex() generates primes p and q and a generator g
for use in the DSA and stores the result in **dsa**.

**bits** is the length of the prime p to be generated.
For lengths under 2048 bits, the length of q is 160 bits; for lengths
greater than or equal to 2048 bits, the length of q is set to 256 bits.

If **seed** is NULL, the primes will be generated at random.
If **seed\_len** is less than the length of q, an error is returned.

DSA\_generate\_parameters\_ex() places the iteration count in
\***counter\_ret** and a counter used for finding a generator in
\***h\_ret**, unless these are **NULL**.

A callback function may be used to provide feedback about the progress
of the key generation. If **cb** is not **NULL**, it will be
called as shown below. For information on the BN\_GENCB structure and the
BN\_GENCB\_call function discussed below, refer to
[BN\_generate\_prime(3)](http://man.he.net/man3/BN_generate_prime).

- When a candidate for q is generated, **BN\_GENCB\_call(cb, 0, m++)** is called
(m is 0 for the first candidate).
- When a candidate for q has passed a test by trial division,
**BN\_GENCB\_call(cb, 1, -1)** is called.
While a candidate for q is tested by Miller-Rabin primality tests,
**BN\_GENCB\_call(cb, 1, i)** is called in the outer loop
(once for each witness that confirms that the candidate may be prime);
i is the loop counter (starting at 0).
- When a prime q has been found, **BN\_GENCB\_call(cb, 2, 0)** and
**BN\_GENCB\_call(cb, 3, 0)** are called.
- Before a candidate for p (other than the first) is generated and tested,
**BN\_GENCB\_call(cb, 0, counter)** is called.
- When a candidate for p has passed the test by trial division,
**BN\_GENCB\_call(cb, 1, -1)** is called.
While it is tested by the Miller-Rabin primality test,
**BN\_GENCB\_call(cb, 1, i)** is called in the outer loop
(once for each witness that confirms that the candidate may be prime).
i is the loop counter (starting at 0).
- When p has been found, **BN\_GENCB\_call(cb, 2, 1)** is called.
- When the generator has been found, **BN\_GENCB\_call(cb, 3, 1)** is called.

DSA\_generate\_parameters() (deprecated) works in much the same way as for DSA\_generate\_parameters\_ex, except that no **dsa** parameter is passed and
instead a newly allocated **DSA** structure is returned. Additionally "old
style" callbacks are used instead of the newer BN\_GENCB based approach.
Refer to [BN\_generate\_prime(3)](http://man.he.net/man3/BN_generate_prime) for further information.

# RETURN VALUE

DSA\_generate\_parameters\_ex() returns a 1 on success, or 0 otherwise.

DSA\_generate\_parameters() returns a pointer to the DSA structure, or
**NULL** if the parameter generation fails.

The error codes can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# BUGS

Seed lengths > 20 are not supported.

# SEE ALSO

[dsa(3)](http://man.he.net/man3/dsa), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [rand(3)](http://man.he.net/man3/rand),
[DSA\_free(3)](http://man.he.net/man3/DSA_free), [BN\_generate\_prime(3)](http://man.he.net/man3/BN_generate_prime)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
