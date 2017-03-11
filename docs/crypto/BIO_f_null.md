# NAME

BIO\_f\_null - null filter

# SYNOPSIS

    #include <openssl/bio.h>

    const BIO_METHOD *     BIO_f_null(void);

# DESCRIPTION

BIO\_f\_null() returns the null filter BIO method. This is a filter BIO
that does nothing.

All requests to a null filter BIO are passed through to the next BIO in
the chain: this means that a BIO chain containing a null filter BIO
behaves just as though the BIO was not there.

# NOTES

As may be apparent a null filter BIO is not particularly useful.

# RETURN VALUES

BIO\_f\_null() returns the null filter BIO method.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
