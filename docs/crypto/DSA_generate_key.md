# NAME

DSA\_generate\_key - generate DSA key pair

# SYNOPSIS

    #include <openssl/dsa.h>

    int DSA_generate_key(DSA *a);

# DESCRIPTION

DSA\_generate\_key() expects **a** to contain DSA parameters. It generates
a new key pair and stores it in **a->pub\_key** and **a->priv\_key**.

The PRNG must be seeded prior to calling DSA\_generate\_key().

# RETURN VALUE

DSA\_generate\_key() returns 1 on success, 0 otherwise.
The error codes can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# SEE ALSO

[dsa(3)](http://man.he.net/man3/dsa), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [rand(3)](http://man.he.net/man3/rand),
[DSA\_generate\_parameters(3)](http://man.he.net/man3/DSA_generate_parameters)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
