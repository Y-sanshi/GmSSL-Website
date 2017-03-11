# NAME

MDC2, MDC2\_Init, MDC2\_Update, MDC2\_Final - MDC2 hash function

# SYNOPSIS

    #include <openssl/mdc2.h>

    unsigned char *MDC2(const unsigned char *d, unsigned long n,
                     unsigned char *md);

    int MDC2_Init(MDC2_CTX *c);
    int MDC2_Update(MDC2_CTX *c, const unsigned char *data,
                     unsigned long len);
    int MDC2_Final(unsigned char *md, MDC2_CTX *c);

# DESCRIPTION

MDC2 is a method to construct hash functions with 128 bit output from
block ciphers.  These functions are an implementation of MDC2 with
DES.

MDC2() computes the MDC2 message digest of the **n**
bytes at **d** and places it in **md** (which must have space for
MDC2\_DIGEST\_LENGTH == 16 bytes of output). If **md** is NULL, the digest
is placed in a static array.

The following functions may be used if the message is not completely
stored in memory:

MDC2\_Init() initializes a **MDC2\_CTX** structure.

MDC2\_Update() can be called repeatedly with chunks of the message to
be hashed (**len** bytes at **data**).

MDC2\_Final() places the message digest in **md**, which must have space
for MDC2\_DIGEST\_LENGTH == 16 bytes of output, and erases the **MDC2\_CTX**.

Applications should use the higher level functions
[EVP\_DigestInit(3)](http://man.he.net/man3/EVP_DigestInit) etc. instead of calling the
hash functions directly.

# RETURN VALUES

MDC2() returns a pointer to the hash value.

MDC2\_Init(), MDC2\_Update() and MDC2\_Final() return 1 for success, 0 otherwise.

# CONFORMING TO

ISO/IEC 10118-2, with DES

# SEE ALSO

[EVP\_DigestInit(3)](http://man.he.net/man3/EVP_DigestInit)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
