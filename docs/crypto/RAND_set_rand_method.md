# NAME

RAND\_set\_rand\_method, RAND\_get\_rand\_method, RAND\_OpenSSL - select RAND method

# SYNOPSIS

    #include <openssl/rand.h>

    void RAND_set_rand_method(const RAND_METHOD *meth);

    const RAND_METHOD *RAND_get_rand_method(void);

    RAND_METHOD *RAND_OpenSSL(void);

# DESCRIPTION

A **RAND\_METHOD** specifies the functions that OpenSSL uses for random number
generation. By modifying the method, alternative implementations such as
hardware RNGs may be used. IMPORTANT: See the NOTES section for important
information about how these RAND API functions are affected by the use of
**ENGINE** API calls.

Initially, the default RAND\_METHOD is the OpenSSL internal implementation, as
returned by RAND\_OpenSSL().

RAND\_set\_default\_method() makes **meth** the method for PRNG use. **NB**: This is
true only whilst no ENGINE has been set as a default for RAND, so this function
is no longer recommended.

RAND\_get\_default\_method() returns a pointer to the current RAND\_METHOD.
However, the meaningfulness of this result is dependent on whether the ENGINE
API is being used, so this function is no longer recommended.

# THE RAND\_METHOD STRUCTURE

    typedef struct rand_meth_st
    {
           void (*seed)(const void *buf, int num);
           int (*bytes)(unsigned char *buf, int num);
           void (*cleanup)(void);
           void (*add)(const void *buf, int num, int entropy);
           int (*pseudorand)(unsigned char *buf, int num);
           int (*status)(void);
    } RAND_METHOD;

The components point to method implementations used by (or called by), in order,
RAND\_seed(), RAND\_bytes(), internal RAND cleanup, RAND\_add(), RAND\_pseudo\_rand()
and RAND\_status().
Each component may be NULL if the function is not implemented.

# RETURN VALUES

RAND\_set\_rand\_method() returns no value. RAND\_get\_rand\_method() and
RAND\_OpenSSL() return pointers to the respective methods.

# NOTES

RAND\_METHOD implementations are grouped together with other
algorithmic APIs (eg. RSA\_METHOD, EVP\_CIPHER, etc) in **ENGINE** modules. If a
default ENGINE is specified for RAND functionality using an ENGINE API function,
that will override any RAND defaults set using the RAND API (ie.
RAND\_set\_rand\_method()). For this reason, the ENGINE API is the recommended way
to control default implementations for use in RAND and other cryptographic
algorithms.

# SEE ALSO

[rand(3)](http://man.he.net/man3/rand), [engine(3)](http://man.he.net/man3/engine)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
