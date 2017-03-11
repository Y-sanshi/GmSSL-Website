# NAME

SSL\_get\_rbio, SSL\_get\_wbio - get BIO linked to an SSL object

# SYNOPSIS

    #include <openssl/ssl.h>

    BIO *SSL_get_rbio(SSL *ssl);
    BIO *SSL_get_wbio(SSL *ssl);

# DESCRIPTION

SSL\_get\_rbio() and SSL\_get\_wbio() return pointers to the BIOs for the
read or the write channel, which can be different. The reference count
of the BIO is not incremented.

# RETURN VALUES

The following return values can occur:

- NULL

    No BIO was connected to the SSL object

- Any other pointer

    The BIO linked to **ssl**.

# SEE ALSO

[SSL\_set\_bio(3)](http://man.he.net/man3/SSL_set_bio), [ssl(3)](http://man.he.net/man3/ssl) , [bio(3)](http://man.he.net/man3/bio)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
