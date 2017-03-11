# NAME

X509\_NAME\_get0\_der - get X509\_NAME DER encoding

# SYNOPSIS

    #include <openssl/x509.h>

    int X509_NAME_get0_der(X509_NAME *nm, const unsigned char **pder,
                           size_t *pderlen)

# DESCRIPTION

The function X509\_NAME\_get0\_der() returns an internal pointer to the
encoding of an **X509\_NAME** structure in **\*pder** and consisting of
**\*pderlen** bytes. It is useful for applications that wish to examine
the encoding of an **X509\_NAME** structure without copying it.

# RETURN VALUES

The function X509\_NAME\_get0\_der() returns 1 for success and 0 if an error
occurred.

# SEE ALSO

[d2i\_X509(3)](http://man.he.net/man3/d2i_X509)

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
