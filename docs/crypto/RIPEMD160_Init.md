# NAME

RIPEMD160, RIPEMD160\_Init, RIPEMD160\_Update, RIPEMD160\_Final -
RIPEMD-160 hash function

# SYNOPSIS

    #include <openssl/ripemd.h>

    unsigned char *RIPEMD160(const unsigned char *d, unsigned long n,
                     unsigned char *md);

    int RIPEMD160_Init(RIPEMD160_CTX *c);
    int RIPEMD160_Update(RIPEMD_CTX *c, const void *data,
                     unsigned long len);
    int RIPEMD160_Final(unsigned char *md, RIPEMD160_CTX *c);

# DESCRIPTION

RIPEMD-160 is a cryptographic hash function with a
160 bit output.

RIPEMD160() computes the RIPEMD-160 message digest of the **n**
bytes at **d** and places it in **md** (which must have space for
RIPEMD160\_DIGEST\_LENGTH == 20 bytes of output). If **md** is NULL, the digest
is placed in a static array.

The following functions may be used if the message is not completely
stored in memory:

RIPEMD160\_Init() initializes a **RIPEMD160\_CTX** structure.

RIPEMD160\_Update() can be called repeatedly with chunks of the message to
be hashed (**len** bytes at **data**).

RIPEMD160\_Final() places the message digest in **md**, which must have
space for RIPEMD160\_DIGEST\_LENGTH == 20 bytes of output, and erases
the **RIPEMD160\_CTX**.

# RETURN VALUES

RIPEMD160() returns a pointer to the hash value.

RIPEMD160\_Init(), RIPEMD160\_Update() and RIPEMD160\_Final() return 1 for
success, 0 otherwise.

# NOTE

Applications should use the higher level functions
[EVP\_DigestInit(3)](http://man.he.net/man3/EVP_DigestInit) etc. instead of calling these
functions directly.

# CONFORMING TO

ISO/IEC 10118-3 (draft) (??)

# SEE ALSO

[EVP\_DigestInit(3)](http://man.he.net/man3/EVP_DigestInit)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
