# NAME

DH\_generate\_key, DH\_compute\_key - perform Diffie-Hellman key exchange

# SYNOPSIS

    #include <openssl/dh.h>

    int DH_generate_key(DH *dh);

    int DH_compute_key(unsigned char *key, BIGNUM *pub_key, DH *dh);

# DESCRIPTION

DH\_generate\_key() performs the first step of a Diffie-Hellman key
exchange by generating private and public DH values. By calling
DH\_compute\_key(), these are combined with the other party's public
value to compute the shared key.

DH\_generate\_key() expects **dh** to contain the shared parameters
**dh->p** and **dh->g**. It generates a random private DH value
unless **dh->priv\_key** is already set, and computes the
corresponding public value **dh->pub\_key**, which can then be
published.

DH\_compute\_key() computes the shared secret from the private DH value
in **dh** and the other party's public value in **pub\_key** and stores
it in **key**. **key** must point to **DH\_size(dh)** bytes of memory.

# RETURN VALUES

DH\_generate\_key() returns 1 on success, 0 otherwise.

DH\_compute\_key() returns the size of the shared secret on success, -1
on error.

The error codes can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# SEE ALSO

[dh(3)](http://man.he.net/man3/dh), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [rand(3)](http://man.he.net/man3/rand), [DH\_size(3)](http://man.he.net/man3/DH_size)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
