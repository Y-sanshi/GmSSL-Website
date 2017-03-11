# NAME

EC\_KEY\_get\_method, EC\_KEY\_set\_method,
EC\_KEY\_new, EC\_KEY\_get\_flags, EC\_KEY\_set\_flags, EC\_KEY\_clear\_flags,
EC\_KEY\_new\_by\_curve\_name, EC\_KEY\_free, EC\_KEY\_copy, EC\_KEY\_dup, EC\_KEY\_up\_ref,
EC\_KEY\_get0\_group, EC\_KEY\_set\_group, EC\_KEY\_get0\_private\_key,
EC\_KEY\_set\_private\_key, EC\_KEY\_get0\_public\_key, EC\_KEY\_set\_public\_key,
EC\_KEY\_get\_conv\_form,
EC\_KEY\_set\_conv\_form, EC\_KEY\_set\_asn1\_flag, EC\_KEY\_precompute\_mult,
EC\_KEY\_generate\_key, EC\_KEY\_check\_key, EC\_KEY\_set\_public\_key\_affine\_coordinates,
EC\_KEY\_oct2key, EC\_KEY\_key2buf, EC\_KEY\_oct2priv, EC\_KEY\_priv2oct,
EC\_KEY\_priv2buf - Functions for creating, destroying and manipulating
EC\_KEY objects

# SYNOPSIS

    #include <openssl/ec.h>

    EC_KEY *EC_KEY_new(void);
    int EC_KEY_get_flags(const EC_KEY *key);
    void EC_KEY_set_flags(EC_KEY *key, int flags);
    void EC_KEY_clear_flags(EC_KEY *key, int flags);
    EC_KEY *EC_KEY_new_by_curve_name(int nid);
    void EC_KEY_free(EC_KEY *key);
    EC_KEY *EC_KEY_copy(EC_KEY *dst, const EC_KEY *src);
    EC_KEY *EC_KEY_dup(const EC_KEY *src);
    int EC_KEY_up_ref(EC_KEY *key);
    const EC_GROUP *EC_KEY_get0_group(const EC_KEY *key);
    int EC_KEY_set_group(EC_KEY *key, const EC_GROUP *group);
    const BIGNUM *EC_KEY_get0_private_key(const EC_KEY *key);
    int EC_KEY_set_private_key(EC_KEY *key, const BIGNUM *prv);
    const EC_POINT *EC_KEY_get0_public_key(const EC_KEY *key);
    int EC_KEY_set_public_key(EC_KEY *key, const EC_POINT *pub);
    point_conversion_form_t EC_KEY_get_conv_form(const EC_KEY *key);
    void EC_KEY_set_conv_form(EC_KEY *eckey, point_conversion_form_t cform);
    void EC_KEY_set_asn1_flag(EC_KEY *eckey, int asn1_flag);
    int EC_KEY_precompute_mult(EC_KEY *key, BN_CTX *ctx);
    int EC_KEY_generate_key(EC_KEY *key);
    int EC_KEY_check_key(const EC_KEY *key);
    int EC_KEY_set_public_key_affine_coordinates(EC_KEY *key,
                                                 BIGNUM *x, BIGNUM *y);
    const EC_KEY_METHOD *EC_KEY_get_method(const EC_KEY *key);
    int EC_KEY_set_method(EC_KEY *key, const EC_KEY_METHOD *meth);

    int EC_KEY_oct2key(EC_KEY *eckey, const unsigned char *buf, size_t len,
                       BN_CTX *ctx);
    size_t EC_KEY_key2buf(const EC_KEY *eckey, point_conversion_form_t form,
                          unsigned char **pbuf, BN_CTX *ctx);

    int EC_KEY_oct2priv(EC_KEY *eckey, const unsigned char *buf, size_t len);
    size_t EC_KEY_priv2oct(const EC_KEY *eckey, unsigned char *buf, size_t len);

    size_t EC_KEY_priv2buf(const EC_KEY *eckey, unsigned char **pbuf);

# DESCRIPTION

An EC\_KEY represents a public key and, optionally, the associated private
key. A new EC\_KEY with no associated curve can be constructed by calling
EC\_KEY\_new(). The reference count for the newly created EC\_KEY is initially
set to 1. A curve can be associated with the EC\_KEY by calling
EC\_KEY\_set\_group().

Alternatively a new EC\_KEY can be constructed by calling
EC\_KEY\_new\_by\_curve\_name() and supplying the nid of the associated curve. See
[EC\_GROUP\_new(3)](http://man.he.net/man3/EC_GROUP_new) for a description of curve names. This function simply
wraps calls to EC\_KEY\_new() and EC\_GROUP\_new\_by\_curve\_name().

Calling EC\_KEY\_free() decrements the reference count for the EC\_KEY object,
and if it has dropped to zero then frees the memory associated with it.  If
**key** is NULL nothing is done.

EC\_KEY\_copy() copies the contents of the EC\_KEY in **src** into **dest**.

EC\_KEY\_dup() creates a new EC\_KEY object and copies **ec\_key** into it.

EC\_KEY\_up\_ref() increments the reference count associated with the EC\_KEY
object.

EC\_KEY\_generate\_key() generates a new public and private key for the supplied
**eckey** object. **eckey** must have an EC\_GROUP object associated with it
before calling this function. The private key is a random integer (0 < priv\_key
< order, where _order_ is the order of the EC\_GROUP object). The public key is
an EC\_POINT on the curve calculated by multiplying the generator for the
curve by the private key.

EC\_KEY\_check\_key() performs various sanity checks on the EC\_KEY object to
confirm that it is valid.

EC\_KEY\_set\_public\_key\_affine\_coordinates() sets the public key for **key** based
on its affine co-ordinates; i.e., it constructs an EC\_POINT object based on
the supplied **x** and **y** values and sets the public key to be this
EC\_POINT. It also performs certain sanity checks on the key to confirm
that it is valid.

The functions EC\_KEY\_get0\_group(), EC\_KEY\_set\_group(),
EC\_KEY\_get0\_private\_key(), EC\_KEY\_set\_private\_key(), EC\_KEY\_get0\_public\_key(),
and EC\_KEY\_set\_public\_key() get and set the EC\_GROUP object, the private key,
and the EC\_POINT public key for the **key** respectively.

The functions EC\_KEY\_get\_conv\_form() and EC\_KEY\_set\_conv\_form() get and set the
point\_conversion\_form for the **key**. For a description of
point\_conversion\_forms please see [EC\_POINT\_new(3)](http://man.he.net/man3/EC_POINT_new).

EC\_KEY\_set\_flags() sets the flags in the **flags** parameter on the EC\_KEY
object. Any flags that are already set are left set. The flags currently
defined are EC\_FLAG\_NON\_FIPS\_ALLOW and EC\_FLAG\_FIPS\_CHECKED. In
addition there is the flag EC\_FLAG\_COFACTOR\_ECDH which is specific to ECDH.
EC\_KEY\_get\_flags() returns the current flags that are set for this EC\_KEY.
EC\_KEY\_clear\_flags() clears the flags indicated by the **flags** parameter; all
other flags are left in their existing state.

EC\_KEY\_set\_asn1\_flag() sets the asn1\_flag on the underlying EC\_GROUP object
(if set). Refer to [EC\_GROUP\_copy(3)](http://man.he.net/man3/EC_GROUP_copy) for further information on the
asn1\_flag.

EC\_KEY\_precompute\_mult() stores multiples of the underlying EC\_GROUP generator
for faster point multiplication. See also [EC\_POINT\_add(3)](http://man.he.net/man3/EC_POINT_add).

EC\_KEY\_oct2key() and EC\_KEY\_key2buf() are identical to the functions
EC\_POINT\_oct2point() and EC\_KEY\_point2buf() except they use the public key
EC\_POINT in **eckey**.

EC\_KEY\_oct2priv() and EC\_KEY\_priv2oct() convert between the private key
component of **eckey** and octet form. The octet form consists of the content
octets of the **privateKey** OCTET STRING in an **ECPrivateKey** ASN.1 structure.

The function EC\_KEY\_priv2oct() must be supplied with a buffer long enough to
store the octet form. The return value provides the number of octets stored.
Calling the function with a NULL buffer will not perform the conversion but
will just return the required buffer length.

The function EC\_KEY\_priv2buf() allocates a buffer of suitable length and writes
an EC\_KEY to it in octet format. The allocated buffer is written to **\*pbuf**
and its length is returned. The caller must free up the allocated buffer with a
call to OPENSSL\_free(). Since the allocated buffer value is written to **\*pbuf**
the **pbuf** parameter **MUST NOT** be **NULL**.

EC\_KEY\_priv2buf() converts an EC\_KEY private key into an allocated buffer.

# RETURN VALUES

EC\_KEY\_new(), EC\_KEY\_new\_by\_curve\_name() and EC\_KEY\_dup() return a pointer to
the newly created EC\_KEY object, or NULL on error.

EC\_KEY\_get\_flags() returns the flags associated with the EC\_KEY object as an
integer.

EC\_KEY\_copy() returns a pointer to the destination key, or NULL on error.

EC\_KEY\_up\_ref(), EC\_KEY\_set\_group(), EC\_KEY\_set\_private\_key(),
EC\_KEY\_set\_public\_key(), EC\_KEY\_precompute\_mult(), EC\_KEY\_generate\_key(),
EC\_KEY\_check\_key(), EC\_KEY\_set\_public\_key\_affine\_coordinates(),
EC\_KEY\_oct2key() and EC\_KEY\_oct2priv() return 1 on success or 0 on error.

EC\_KEY\_get0\_group() returns the EC\_GROUP associated with the EC\_KEY.

EC\_KEY\_get0\_private\_key() returns the private key associated with the EC\_KEY.

EC\_KEY\_get\_conv\_form() return the point\_conversion\_form for the EC\_KEY.

EC\_KEY\_key2buf(), EC\_KEY\_priv2oct() and EC\_KEY\_priv2buf() return the length
of the buffer or 0 on error.

# SEE ALSO

[crypto(3)](http://man.he.net/man3/crypto), [ec(3)](http://man.he.net/man3/ec), [EC\_GROUP\_new(3)](http://man.he.net/man3/EC_GROUP_new),
[EC\_GROUP\_copy(3)](http://man.he.net/man3/EC_GROUP_copy), [EC\_POINT\_new(3)](http://man.he.net/man3/EC_POINT_new),
[EC\_POINT\_add(3)](http://man.he.net/man3/EC_POINT_add),
[EC\_GFp\_simple\_method(3)](http://man.he.net/man3/EC_GFp_simple_method),
[d2i\_ECPKParameters(3)](http://man.he.net/man3/d2i_ECPKParameters)

# COPYRIGHT

Copyright 2013-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
