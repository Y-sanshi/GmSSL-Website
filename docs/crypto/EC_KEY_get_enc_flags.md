# NAME

EC\_KEY\_get\_enc\_flags, EC\_KEY\_set\_enc\_flags
\- Get and set flags for encoding EC\_KEY structures

# SYNOPSIS

    #include <openssl/ec.h>

    unsigned int EC_KEY_get_enc_flags(const EC_KEY *key);
    void EC_KEY_set_enc_flags(EC_KEY *eckey, unsigned int flags);

# DESCRIPTION

The format of the external representation of the public key written by
i2d\_ECPrivateKey() (such as whether it is stored in a compressed form or not) is
described by the point\_conversion\_form. See [EC\_GROUP\_copy(3)](http://man.he.net/man3/EC_GROUP_copy)
for a description of point\_conversion\_form.

When reading a private key encoded without an associated public key (e.g. if
EC\_PKEY\_NO\_PUBKEY has been used - see below), then d2i\_ECPrivateKey() generates
the missing public key automatically. Private keys encoded without parameters
(e.g. if EC\_PKEY\_NO\_PARAMETERS has been used - see below) cannot be loaded using
d2i\_ECPrivateKey().

The functions EC\_KEY\_get\_enc\_flags() and EC\_KEY\_set\_enc\_flags() get and set the
value of the encoding flags for the **key**. There are two encoding flags
currently defined - EC\_PKEY\_NO\_PARAMETERS and EC\_PKEY\_NO\_PUBKEY.  These flags
define the behaviour of how the  **key** is converted into ASN1 in a call to
i2d\_ECPrivateKey(). If EC\_PKEY\_NO\_PARAMETERS is set then the public parameters for
the curve are not encoded along with the private key. If EC\_PKEY\_NO\_PUBKEY is
set then the public key is not encoded along with the private key.

# RETURN VALUES

EC\_KEY\_get\_enc\_flags() returns the value of the current encoding flags for the
EC\_KEY.

# SEE ALSO

[crypto(3)](http://man.he.net/man3/crypto), [ec(3)](http://man.he.net/man3/ec), [EC\_GROUP\_new(3)](http://man.he.net/man3/EC_GROUP_new),
[EC\_GROUP\_copy(3)](http://man.he.net/man3/EC_GROUP_copy), [EC\_POINT\_new(3)](http://man.he.net/man3/EC_POINT_new),
[EC\_POINT\_add(3)](http://man.he.net/man3/EC_POINT_add),
[EC\_GFp\_simple\_method(3)](http://man.he.net/man3/EC_GFp_simple_method),
[d2i\_ECPKParameters(3)](http://man.he.net/man3/d2i_ECPKParameters),
[d2i\_ECPrivateKey(3)](http://man.he.net/man3/d2i_ECPrivateKey)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
