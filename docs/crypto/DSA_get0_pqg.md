# NAME

DSA\_get0\_pqg, DSA\_set0\_pqg, DSA\_get0\_key, DSA\_set0\_key, DSA\_clear\_flags,
DSA\_test\_flags, DSA\_set\_flags, DSA\_get0\_engine - Routines for getting and
setting data in a DSA object

# SYNOPSIS

    #include <openssl/dsa.h>

    void DSA_get0_pqg(const DSA *d,
                      const BIGNUM **p, const BIGNUM **q, const BIGNUM **g);
    int DSA_set0_pqg(DSA *d, BIGNUM *p, BIGNUM *q, BIGNUM *g);
    void DSA_get0_key(const DSA *d,
                      const BIGNUM **pub_key, const BIGNUM **priv_key);
    int DSA_set0_key(DSA *d, BIGNUM *pub_key, BIGNUM *priv_key);
    void DSA_clear_flags(DSA *d, int flags);
    int DSA_test_flags(const DSA *d, int flags);
    void DSA_set_flags(DSA *d, int flags);
    ENGINE *DSA_get0_engine(DSA *d);

# DESCRIPTION

A DSA object contains the parameters **p**, **q** and **g**. It also contains a
public key (**pub\_key**) and (optionally) a private key (**priv\_key**).

The **p**, **q** and **g** parameters can be obtained by calling DSA\_get0\_pqg().
If the parameters have not yet been set then **\*p**, **\*q** and **\*g** will be set
to NULL. Otherwise they are set to pointers to their respective values. These
point directly to the internal representations of the values and therefore
should not be freed directly.

The **p**, **q** and **g** values can be set by calling DSA\_set0\_pqg() and passing
the new values for **p**, **q** and **g** as parameters to the function. Calling
this function transfers the memory management of the values to the DSA object,
and therefore the values that have been passed in should not be freed directly
after this function has been called.

To get the public and private key values use the DSA\_get0\_key() function. A
pointer to the public key will be stored in **\*pub\_key**, and a pointer to the
private key will be stored in **\*priv\_key**. Either may be NULL if they have not
been set yet, although if the private key has been set then the public key must
be. The values point to the internal representation of the public key and
private key values. This memory should not be freed directly.

The public and private key values can be set using DSA\_set0\_key(). The public
key must be non-NULL the first time this function is called on a given DSA
object. The private key may be NULL.  On subsequent calls, either may be NULL,
which means the corresponding DSA field is left untouched. As for DSA\_set0\_pqg()
this function transfers the memory management of the key values to the DSA
object, and therefore they should not be freed directly after this function has
been called.

DSA\_set\_flags() sets the flags in the **flags** parameter on the DSA object.
Multiple flags can be passed in one go (bitwise ORed together). Any flags that
are already set are left set. DSA\_test\_flags() tests to see whether the flags
passed in the **flags** parameter are currently set in the DSA object. Multiple
flags can be tested in one go. All flags that are currently set are returned, or
zero if none of the flags are set. DSA\_clear\_flags() clears the specified flags
within the DSA object.

DSA\_get0\_engine() returns a handle to the ENGINE that has been set for this DSA
object, or NULL if no such ENGINE has been set.

# NOTES

Values retrieved with DSA\_get0\_key() are owned by the DSA object used
in the call and may therefore _not_ be passed to DSA\_set0\_key().  If
needed, duplicate the received value using BN\_dup() and pass the
duplicate.  The same applies to DSA\_get0\_pqg() and DSA\_set0\_pqg().

# RETURN VALUES

DSA\_set0\_pqg() and DSA\_set0\_key() return 1 on success or 0 on failure.

DSA\_test\_flags() returns the current state of the flags in the DSA object.

DSA\_get0\_engine() returns the ENGINE set for the DSA object or NULL if no ENGINE
has been set.

# SEE ALSO

[dsa(3)](http://man.he.net/man3/dsa), [DSA\_new(3)](http://man.he.net/man3/DSA_new), [DSA\_generate\_parameters(3)](http://man.he.net/man3/DSA_generate_parameters), [DSA\_generate\_key(3)](http://man.he.net/man3/DSA_generate_key),
[DSA\_dup\_DH(3)](http://man.he.net/man3/DSA_dup_DH), [DSA\_do\_sign(3)](http://man.he.net/man3/DSA_do_sign), [DSA\_set\_method(3)](http://man.he.net/man3/DSA_set_method), [DSA\_SIG\_new(3)](http://man.he.net/man3/DSA_SIG_new),
[DSA\_sign(3)](http://man.he.net/man3/DSA_sign), [DSA\_size(3)](http://man.he.net/man3/DSA_size), [DSA\_meth\_new(3)](http://man.he.net/man3/DSA_meth_new)

# HISTORY

The functions described here were added in OpenSSL version 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
