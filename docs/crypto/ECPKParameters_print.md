# NAME

ECPKParameters\_print, ECPKParameters\_print\_fp - Functions for decoding and
encoding ASN1 representations of elliptic curve entities

# SYNOPSIS

    #include <openssl/ec.h>

    int ECPKParameters_print(BIO *bp, const EC_GROUP *x, int off);
    int ECPKParameters_print_fp(FILE *fp, const EC_GROUP *x, int off);

# DESCRIPTION

The ECPKParameters represent the public parameters for an
**EC\_GROUP** structure, which represents a curve.

The ECPKParameters\_print() and ECPKParameters\_print\_fp() functions print
a human-readable output of the public parameters of the EC\_GROUP to **bp**
or **fp**. The output lines are indented by **off** spaces.

# RETURN VALUES

ECPKParameters\_print() and ECPKParameters\_print\_fp()
return 1 for success and 0 if an error occurs.

# SEE ALSO

[crypto(3)](http://man.he.net/man3/crypto), [ec(3)](http://man.he.net/man3/ec), [EC\_GROUP\_new(3)](http://man.he.net/man3/EC_GROUP_new), [EC\_GROUP\_copy(3)](http://man.he.net/man3/EC_GROUP_copy),
[EC\_POINT\_new(3)](http://man.he.net/man3/EC_POINT_new), [EC\_POINT\_add(3)](http://man.he.net/man3/EC_POINT_add), [EC\_KEY\_new(3)](http://man.he.net/man3/EC_KEY_new),
[EC\_GFp\_simple\_method(3)](http://man.he.net/man3/EC_GFp_simple_method),

# COPYRIGHT

Copyright 2013-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
