# NAME

i2d\_Netscape\_RSA,
d2i\_Netscape\_RSA
\- insecure RSA public and private key encoding functions

# SYNOPSIS

    #include <openssl/rsa.h>

    int i2d_Netscape_RSA(RSA *a, unsigned char **pp, int (*cb)());
    RSA * d2i_Netscape_RSA(RSA **a, const unsigned char **pp, long length, int (*cb)());

# DESCRIPTION

These functions decode and encode an RSA private
key in NET format.  These functions are present to provide compatibility
with very old software. This format has some severe security weaknesses
and should be avoided if possible.

These functions are similar to the **d2i\_RSAPrivateKey** functions.

# SEE ALSO

[d2i\_RSAPrivateKey(3)](http://man.he.net/man3/d2i_RSAPrivateKey)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
