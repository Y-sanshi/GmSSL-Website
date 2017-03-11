# NAME

RSA\_print, RSA\_print\_fp,
DSAparams\_print, DSAparams\_print\_fp, DSA\_print, DSA\_print\_fp,
DHparams\_print, DHparams\_print\_fp - print cryptographic parameters

# SYNOPSIS

    #include <openssl/rsa.h>

    int RSA_print(BIO *bp, RSA *x, int offset);
    int RSA_print_fp(FILE *fp, RSA *x, int offset);

    #include <openssl/dsa.h>

    int DSAparams_print(BIO *bp, DSA *x);
    int DSAparams_print_fp(FILE *fp, DSA *x);
    int DSA_print(BIO *bp, DSA *x, int offset);
    int DSA_print_fp(FILE *fp, DSA *x, int offset);

    #include <openssl/dh.h>

    int DHparams_print(BIO *bp, DH *x);
    int DHparams_print_fp(FILE *fp, DH *x);

# DESCRIPTION

A human-readable hexadecimal output of the components of the RSA
key, DSA parameters or key or DH parameters is printed to **bp** or **fp**.

The output lines are indented by **offset** spaces.

# RETURN VALUES

These functions return 1 on success, 0 on error.

# SEE ALSO

[BN\_bn2bin(3)](http://man.he.net/man3/BN_bn2bin)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
