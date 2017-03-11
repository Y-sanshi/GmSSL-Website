# NAME

DSA\_size, DSA\_bits - get DSA signature size or key bits

# SYNOPSIS

    #include <openssl/dsa.h>

    int DSA_size(const DSA *dsa);
    int DSA_bits(const DSA *dsa);

# DESCRIPTION

DSA\_size() returns the maximum size of an ASN.1 encoded DSA signature
for key **dsa** in bytes. It can be used to determine how much memory must
be allocated for a DSA signature.

**dsa->q** must not be **NULL**.

DSA\_bits() returns the number of bits in key **dsa**: this is the number
of bits in the **p** parameter.

# RETURN VALUE

DSA\_size() returns the size in bytes.

DSA\_bits() returns the number of bits in the key.

# SEE ALSO

[dsa(3)](http://man.he.net/man3/dsa), [DSA\_sign(3)](http://man.he.net/man3/DSA_sign)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
