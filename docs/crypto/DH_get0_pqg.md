# NAME

DH\_get0\_pqg, DH\_set0\_pqg, DH\_get0\_key, DH\_set0\_key, DH\_clear\_flags,
DH\_test\_flags, DH\_set\_flags, DH\_get0\_engine, DH\_get\_length,
DH\_set\_length - Routines for getting and setting data in a DH object

# SYNOPSIS

    #include <openssl/dh.h>

    void DH_get0_pqg(const DH *dh,
                     const BIGNUM **p, const BIGNUM **q, const BIGNUM **g);
    int DH_set0_pqg(DH *dh, BIGNUM *p, BIGNUM *q, BIGNUM *g);
    void DH_get0_key(const DH *dh,
                     const BIGNUM **pub_key, const BIGNUM **priv_key);
    int DH_set0_key(DH *dh, BIGNUM *pub_key, BIGNUM *priv_key);
    void DH_clear_flags(DH *dh, int flags);
    int DH_test_flags(const DH *dh, int flags);
    void DH_set_flags(DH *dh, int flags);
    ENGINE *DH_get0_engine(DH *d);
    long DH_get_length(const DH *dh);
    int DH_set_length(DH *dh, long length);

# DESCRIPTION

A DH object contains the parameters **p**, **q** and **g**. Note that the **q**
parameter is optional. It also contains a public key (**pub\_key**) and
(optionally) a private key (**priv\_key**).

The **p**, **q** and **g** parameters can be obtained by calling DH\_get0\_pqg().
If the parameters have not yet been set then **\*p**, **\*q** and **\*g** will be set
to NULL. Otherwise they are set to pointers to their respective values. These
point directly to the internal representations of the values and therefore
should not be freed directly.

The **p**, **q** and **g** values can be set by calling DH\_set0\_pqg() and passing
the new values for **p**, **q** and **g** as parameters to the function. Calling
this function transfers the memory management of the values to the DH object,
and therefore the values that have been passed in should not be freed directly
after this function has been called. The **q** parameter may be NULL.

To get the public and private key values use the DH\_get0\_key() function. A
pointer to the public key will be stored in **\*pub\_key**, and a pointer to the
private key will be stored in **\*priv\_key**. Either may be NULL if they have not
been set yet, although if the private key has been set then the public key must
be. The values point to the internal representation of the public key and
private key values. This memory should not be freed directly.

The public and private key values can be set using DH\_set0\_key(). The public
key must be non-NULL the first time this function is called on a given DH
object. The private key may be NULL.  On subsequent calls, either may be NULL,
which means the corresponding DH field is left untouched. As for DH\_set0\_pqg()
this function transfers the memory management of the key values to the DH
object, and therefore they should not be freed directly after this function has
been called.

DH\_set\_flags() sets the flags in the **flags** parameter on the DH object.
Multiple flags can be passed in one go (bitwise ORed together). Any flags that
are already set are left set. DH\_test\_flags() tests to see whether the flags
passed in the **flags** parameter are currently set in the DH object. Multiple
flags can be tested in one go. All flags that are currently set are returned, or
zero if none of the flags are set. DH\_clear\_flags() clears the specified flags
within the DH object.

DH\_get0\_engine() returns a handle to the ENGINE that has been set for this DH
object, or NULL if no such ENGINE has been set.

The DH\_get\_length() and DH\_set\_length() functions get and set the optional
length parameter associated with this DH object. If the length is non-zero then
it is used, otherwise it is ignored. The **length** parameter indicates the
length of the secret exponent (private key) in bits.

# NOTES

Values retrieved with DH\_get0\_key() are owned by the DH object used
in the call and may therefore _not_ be passed to DH\_set0\_key().  If
needed, duplicate the received value using BN\_dup() and pass the
duplicate.  The same applies to DH\_get0\_pqg() and DH\_set0\_pqg().

# RETURN VALUES

DH\_set0\_pqg() and DH\_set0\_key() return 1 on success or 0 on failure.

DH\_test\_flags() returns the current state of the flags in the DH object.

DH\_get0\_engine() returns the ENGINE set for the DH object or NULL if no ENGINE
has been set.

DH\_get\_length() returns the length of the secret exponent (private key) in bits,
or zero if no such length has been explicitly set.

# SEE ALSO

[dh(3)](http://man.he.net/man3/dh), [DH\_new(3)](http://man.he.net/man3/DH_new), [DH\_generate\_parameters(3)](http://man.he.net/man3/DH_generate_parameters), [DH\_generate\_key(3)](http://man.he.net/man3/DH_generate_key),
[DH\_set\_method(3)](http://man.he.net/man3/DH_set_method), [DH\_size(3)](http://man.he.net/man3/DH_size), [DH\_meth\_new(3)](http://man.he.net/man3/DH_meth_new)

# HISTORY

The functions described here were added in OpenSSL version 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
