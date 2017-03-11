# NAME

EC\_GFp\_simple\_method, EC\_GFp\_mont\_method, EC\_GFp\_nist\_method, EC\_GFp\_nistp224\_method, EC\_GFp\_nistp256\_method, EC\_GFp\_nistp521\_method, EC\_GF2m\_simple\_method, EC\_METHOD\_get\_field\_type - Functions for obtaining EC\_METHOD objects

# SYNOPSIS

    #include <openssl/ec.h>

    const EC_METHOD *EC_GFp_simple_method(void);
    const EC_METHOD *EC_GFp_mont_method(void);
    const EC_METHOD *EC_GFp_nist_method(void);
    const EC_METHOD *EC_GFp_nistp224_method(void);
    const EC_METHOD *EC_GFp_nistp256_method(void);
    const EC_METHOD *EC_GFp_nistp521_method(void);

    const EC_METHOD *EC_GF2m_simple_method(void);

    int EC_METHOD_get_field_type(const EC_METHOD *meth);

# DESCRIPTION

The Elliptic Curve library provides a number of different implementations through a single common interface.
When constructing a curve using EC\_GROUP\_new (see [EC\_GROUP\_new(3)](http://man.he.net/man3/EC_GROUP_new)) an
implementation method must be provided. The functions described here all return a const pointer to an
**EC\_METHOD** structure that can be passed to EC\_GROUP\_NEW. It is important that the correct implementation
type for the form of curve selected is used.

For F2^m curves there is only one implementation choice, i.e. EC\_GF2\_simple\_method.

For Fp curves the lowest common denominator implementation is the EC\_GFp\_simple\_method implementation. All
other implementations are based on this one. EC\_GFp\_mont\_method builds on EC\_GFp\_simple\_method but adds the
use of montgomery multiplication (see [BN\_mod\_mul\_montgomery(3)](http://man.he.net/man3/BN_mod_mul_montgomery)). EC\_GFp\_nist\_method
offers an implementation optimised for use with NIST recommended curves (NIST curves are available through
EC\_GROUP\_new\_by\_curve\_name as described in [EC\_GROUP\_new(3)](http://man.he.net/man3/EC_GROUP_new)).

The functions EC\_GFp\_nistp224\_method, EC\_GFp\_nistp256\_method and EC\_GFp\_nistp521\_method offer 64 bit
optimised implementations for the NIST P224, P256 and P521 curves respectively. Note, however, that these
implementations are not available on all platforms.

EC\_METHOD\_get\_field\_type identifies what type of field the EC\_METHOD structure supports, which will be either
F2^m or Fp. If the field type is Fp then the value **NID\_X9\_62\_prime\_field** is returned. If the field type is
F2^m then the value **NID\_X9\_62\_characteristic\_two\_field** is returned. These values are defined in the
obj\_mac.h header file.

# RETURN VALUES

All EC\_GFp\* functions and EC\_GF2m\_simple\_method always return a const pointer to an EC\_METHOD structure.

EC\_METHOD\_get\_field\_type returns an integer that identifies the type of field the EC\_METHOD structure supports.

# SEE ALSO

[crypto(3)](http://man.he.net/man3/crypto), [ec(3)](http://man.he.net/man3/ec), [EC\_GROUP\_new(3)](http://man.he.net/man3/EC_GROUP_new), [EC\_GROUP\_copy(3)](http://man.he.net/man3/EC_GROUP_copy),
[EC\_POINT\_new(3)](http://man.he.net/man3/EC_POINT_new), [EC\_POINT\_add(3)](http://man.he.net/man3/EC_POINT_add), [EC\_KEY\_new(3)](http://man.he.net/man3/EC_KEY_new),
[d2i\_ECPKParameters(3)](http://man.he.net/man3/d2i_ECPKParameters),
[BN\_mod\_mul\_montgomery(3)](http://man.he.net/man3/BN_mod_mul_montgomery)

# COPYRIGHT

Copyright 2013-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
