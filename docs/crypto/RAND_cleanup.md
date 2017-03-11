# NAME

RAND\_cleanup - erase the PRNG state

# SYNOPSIS

    #include <openssl/rand.h>

    #if OPENSSL_API_COMPAT < 0x10100000L
    void RAND_cleanup(void)
    #endif

# DESCRIPTION

Prior to OpenSSL 1.1.0 RAND\_cleanup() erases the memory used by the PRNG. This
function is deprecated and as of version 1.1.0 does nothing. No explicit
initialisation or de-initialisation is necessary. See [OPENSSL\_init\_crypto(3)](http://man.he.net/man3/OPENSSL_init_crypto).

# RETURN VALUE

RAND\_cleanup() returns no value.

# SEE ALSO

[rand(3)](http://man.he.net/man3/rand)

# HISTORY

RAND\_cleanup() was deprecated in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
