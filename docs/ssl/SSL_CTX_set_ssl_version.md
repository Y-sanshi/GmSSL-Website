# NAME

SSL\_CTX\_set\_ssl\_version, SSL\_set\_ssl\_method, SSL\_get\_ssl\_method
\- choose a new TLS/SSL method

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CTX_set_ssl_version(SSL_CTX *ctx, const SSL_METHOD *method);
    int SSL_set_ssl_method(SSL *s, const SSL_METHOD *method);
    const SSL_METHOD *SSL_get_ssl_method(SSL *ssl);

# DESCRIPTION

SSL\_CTX\_set\_ssl\_version() sets a new default TLS/SSL **method** for SSL objects
newly created from this **ctx**. SSL objects already created with
[SSL\_new(3)](http://man.he.net/man3/SSL_new) are not affected, except when
[SSL\_clear(3)](http://man.he.net/man3/SSL_clear) is being called.

SSL\_set\_ssl\_method() sets a new TLS/SSL **method** for a particular **ssl**
object. It may be reset, when SSL\_clear() is called.

SSL\_get\_ssl\_method() returns a function pointer to the TLS/SSL method
set in **ssl**.

# NOTES

The available **method** choices are described in
[SSL\_CTX\_new(3)](http://man.he.net/man3/SSL_CTX_new).

When [SSL\_clear(3)](http://man.he.net/man3/SSL_clear) is called and no session is connected to
an SSL object, the method of the SSL object is reset to the method currently
set in the corresponding SSL\_CTX object.

# RETURN VALUES

The following return values can occur for SSL\_CTX\_set\_ssl\_version()
and SSL\_set\_ssl\_method():

- 0

    The new choice failed, check the error stack to find out the reason.

- 1

    The operation succeeded.

# SEE ALSO

[SSL\_CTX\_new(3)](http://man.he.net/man3/SSL_CTX_new), [SSL\_new(3)](http://man.he.net/man3/SSL_new),
[SSL\_clear(3)](http://man.he.net/man3/SSL_clear), [ssl(3)](http://man.he.net/man3/ssl),
[SSL\_set\_connect\_state(3)](http://man.he.net/man3/SSL_set_connect_state)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
