# NAME

d2i\_DHparams, i2d\_DHparams - PKCS#3 DH parameter functions

# SYNOPSIS

    #include <openssl/dh.h>

    DH *d2i_DHparams(DH **a, unsigned char **pp, long length);
    int i2d_DHparams(DH *a, unsigned char **pp);

# DESCRIPTION

These functions decode and encode PKCS#3 DH parameters using the
DHparameter structure described in PKCS#3.

Otherwise these behave in a similar way to d2i\_X509() and i2d\_X509()
described in the [d2i\_X509(3)](http://man.he.net/man3/d2i_X509) manual page.

# SEE ALSO

[d2i\_X509(3)](http://man.he.net/man3/d2i_X509)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
