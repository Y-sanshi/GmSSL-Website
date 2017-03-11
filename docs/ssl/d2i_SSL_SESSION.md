# NAME

d2i\_SSL\_SESSION, i2d\_SSL\_SESSION - convert SSL\_SESSION object from/to ASN1 representation

# SYNOPSIS

    #include <openssl/ssl.h>

    SSL_SESSION *d2i_SSL_SESSION(SSL_SESSION **a, const unsigned char **pp, long length);
    int i2d_SSL_SESSION(SSL_SESSION *in, unsigned char **pp);

# DESCRIPTION

These functions decode and encode an SSL\_SESSION object.
For encoding details see [d2i\_X509(3)](http://man.he.net/man3/d2i_X509).

SSL\_SESSION objects keep internal link information about the session cache
list, when being inserted into one SSL\_CTX object's session cache.
One SSL\_SESSION object, regardless of its reference count, must therefore
only be used with one SSL\_CTX object (and the SSL objects created
from this SSL\_CTX object).

# RETURN VALUES

d2i\_SSL\_SESSION() returns a pointer to the newly allocated SSL\_SESSION
object. In case of failure the NULL-pointer is returned and the error message
can be retrieved from the error stack.

i2d\_SSL\_SESSION() returns the size of the ASN1 representation in bytes.
When the session is not valid, **0** is returned and no operation is performed.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_SESSION\_free(3)](http://man.he.net/man3/SSL_SESSION_free),
[SSL\_CTX\_sess\_set\_get\_cb(3)](http://man.he.net/man3/SSL_CTX_sess_set_get_cb),
[d2i\_X509(3)](http://man.he.net/man3/d2i_X509)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
