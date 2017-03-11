# NAME

SSL\_get\_SSL\_CTX - get the SSL\_CTX from which an SSL is created

# SYNOPSIS

    #include <openssl/ssl.h>

    SSL_CTX *SSL_get_SSL_CTX(const SSL *ssl);

# DESCRIPTION

SSL\_get\_SSL\_CTX() returns a pointer to the SSL\_CTX object, from which
**ssl** was created with [SSL\_new(3)](http://man.he.net/man3/SSL_new).

# RETURN VALUES

The pointer to the SSL\_CTX object is returned.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_new(3)](http://man.he.net/man3/SSL_new)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
