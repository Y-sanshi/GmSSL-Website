# NAME

SSL\_new, SSL\_up\_ref - create a new SSL structure for a connection

# SYNOPSIS

    #include <openssl/ssl.h>

    SSL *SSL_new(SSL_CTX *ctx);
    int SSL_up_ref(SSL *s);

# DESCRIPTION

SSL\_new() creates a new **SSL** structure which is needed to hold the
data for a TLS/SSL connection. The new structure inherits the settings
of the underlying context **ctx**: connection method,
options, verification settings, timeout settings. An **SSL** structure is
reference counted. Creating an **SSL** structure for the first time increments
the reference count. Freeing it (using SSL\_free) decrements it. When the
reference count drops to zero, any memory or resources allocated to the **SSL**
structure are freed. SSL\_up\_ref() increments the reference count for an
existing **SSL** structure.

# RETURN VALUES

The following return values can occur:

- NULL

    The creation of a new SSL structure failed. Check the error stack to
    find out the reason.

- Pointer to an SSL structure

    The return value points to an allocated SSL structure.

    SSL\_up\_ref() returns 1 for success and 0 for failure.

# SEE ALSO

[SSL\_free(3)](http://man.he.net/man3/SSL_free), [SSL\_clear(3)](http://man.he.net/man3/SSL_clear),
[SSL\_CTX\_set\_options(3)](http://man.he.net/man3/SSL_CTX_set_options),
[SSL\_get\_SSL\_CTX(3)](http://man.he.net/man3/SSL_get_SSL_CTX),
[ssl(3)](http://man.he.net/man3/ssl)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
