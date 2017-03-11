# NAME

RSA\_set0\_key, RSA\_set0\_factors, RSA\_set0\_crt\_params, RSA\_get0\_key,
RSA\_get0\_factors, RSA\_get0\_crt\_params, RSA\_clear\_flags,
RSA\_test\_flags, RSA\_set\_flags, RSA\_get0\_engine - Routines for getting
and setting data in an RSA object

# SYNOPSIS

    #include <openssl/rsa.h>

    int RSA_set0_key(RSA *r, BIGNUM *n, BIGNUM *e, BIGNUM *d);
    int RSA_set0_factors(RSA *r, BIGNUM *p, BIGNUM *q);
    int RSA_set0_crt_params(RSA *r, BIGNUM *dmp1, BIGNUM *dmq1, BIGNUM *iqmp);
    void RSA_get0_key(const RSA *r,
                      const BIGNUM **n, const BIGNUM **e, const BIGNUM **d);
    void RSA_get0_factors(const RSA *r, const BIGNUM **p, const BIGNUM **q);
    void RSA_get0_crt_params(const RSA *r,
                             const BIGNUM **dmp1, const BIGNUM **dmq1,
                             const BIGNUM **iqmp);
    void RSA_clear_flags(RSA *r, int flags);
    int RSA_test_flags(const RSA *r, int flags);
    void RSA_set_flags(RSA *r, int flags);
    ENGINE *RSA_get0_engine(RSA *r);

# DESCRIPTION

An RSA object contains the components for the public and private key,
**n**, **e**, **d**, **p**, **q**, **dmp1**, **dmq1** and **iqmp**.  **n** is
the modulus common to both public and private key, **e** is the public
exponent and **d** is the private exponent.  **p**, **q**, **dmp1**,
**dmq1** and **iqmp** are the factors for the second representation of a
private key (see PKCS#1 section 3 Key Types), where **p** and **q** are
the first and second factor of **n** and **dmp1**, **dmq1** and **iqmp**
are the exponents and coefficient for CRT calculations.

The **n**, **e** and **d** parameters can be obtained by calling
RSA\_get0\_key().  If they have not been set yet, then **\*n**, **\*e** and
**\*d** will be set to NULL.  Otherwise, they are set to pointers to
their respective values. These point directly to the internal
representations of the values and therefore should not be freed
by the caller.

The **n**, **e** and **d** parameter values can be set by calling
RSA\_set0\_key() and passing the new values for **n**, **e** and **d** as
parameters to the function.  The values **n** and **e** must be non-NULL
the first time this function is called on a given RSA object. The
value **d** may be NULL. On subsequent calls any of these values may be
NULL which means the corresponding RSA field is left untouched.
Calling this function transfers the memory management of the values to
the RSA object, and therefore the values that have been passed in
should not be freed by the caller after this function has been called.

In a similar fashion, the **p** and **q** parameters can be obtained and
set with RSA\_get0\_factors() and RSA\_set0\_factors(), and the **dmp1**,
**dmq1** and **iqmp** parameters can be obtained and set with
RSA\_get0\_crt\_params() and RSA\_set0\_crt\_params().

RSA\_set\_flags() sets the flags in the **flags** parameter on the RSA
object. Multiple flags can be passed in one go (bitwise ORed together).
Any flags that are already set are left set. RSA\_test\_flags() tests to
see whether the flags passed in the **flags** parameter are currently
set in the RSA object. Multiple flags can be tested in one go. All
flags that are currently set are returned, or zero if none of the
flags are set. RSA\_clear\_flags() clears the specified flags within the
RSA object.

RSA\_get0\_engine() returns a handle to the ENGINE that has been set for
this RSA object, or NULL if no such ENGINE has been set.

# NOTES

Values retrieved with RSA\_get0\_key() are owned by the RSA object used
in the call and may therefore _not_ be passed to RSA\_set0\_key().  If
needed, duplicate the received value using BN\_dup() and pass the
duplicate.  The same applies to RSA\_get0\_factors() and RSA\_set0\_factors()
as well as RSA\_get0\_crt\_params() and RSA\_set0\_crt\_params().

# RETURN VALUES

RSA\_set0\_key(), RSA\_set0\_factors and RSA\_set0\_crt\_params() return 1 on
success or 0 on failure.

RSA\_test\_flags() returns the current state of the flags in the RSA object.

RSA\_get0\_engine() returns the ENGINE set for the RSA object or NULL if no
ENGINE has been set.

# SEE ALSO

[rsa(3)](http://man.he.net/man3/rsa), [RSA\_new(3)](http://man.he.net/man3/RSA_new), [RSA\_size(3)](http://man.he.net/man3/RSA_size)

# HISTORY

The functions described here were added in OpenSSL version 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
