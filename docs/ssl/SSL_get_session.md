# NAME

SSL\_get\_session, SSL\_get0\_session, SSL\_get1\_session - retrieve TLS/SSL session data

# SYNOPSIS

    #include <openssl/ssl.h>

    SSL_SESSION *SSL_get_session(const SSL *ssl);
    SSL_SESSION *SSL_get0_session(const SSL *ssl);
    SSL_SESSION *SSL_get1_session(SSL *ssl);

# DESCRIPTION

SSL\_get\_session() returns a pointer to the **SSL\_SESSION** actually used in
**ssl**. The reference count of the **SSL\_SESSION** is not incremented, so
that the pointer can become invalid by other operations.

SSL\_get0\_session() is the same as SSL\_get\_session().

SSL\_get1\_session() is the same as SSL\_get\_session(), but the reference
count of the **SSL\_SESSION** is incremented by one.

# NOTES

The ssl session contains all information required to re-establish the
connection without a new handshake.

SSL\_get0\_session() returns a pointer to the actual session. As the
reference counter is not incremented, the pointer is only valid while
the connection is in use. If [SSL\_clear(3)](http://man.he.net/man3/SSL_clear) or
[SSL\_free(3)](http://man.he.net/man3/SSL_free) is called, the session may be removed completely
(if considered bad), and the pointer obtained will become invalid. Even
if the session is valid, it can be removed at any time due to timeout
during [SSL\_CTX\_flush\_sessions(3)](http://man.he.net/man3/SSL_CTX_flush_sessions).

If the data is to be kept, SSL\_get1\_session() will increment the reference
count, so that the session will not be implicitly removed by other operations
but stays in memory. In order to remove the session
[SSL\_SESSION\_free(3)](http://man.he.net/man3/SSL_SESSION_free) must be explicitly called once
to decrement the reference count again.

SSL\_SESSION objects keep internal link information about the session cache
list, when being inserted into one SSL\_CTX object's session cache.
One SSL\_SESSION object, regardless of its reference count, must therefore
only be used with one SSL\_CTX object (and the SSL objects created
from this SSL\_CTX object).

# RETURN VALUES

The following return values can occur:

- NULL

    There is no session available in **ssl**.

- Pointer to an SSL\_SESSION

    The return value points to the data of an SSL session.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_free(3)](http://man.he.net/man3/SSL_free),
[SSL\_clear(3)](http://man.he.net/man3/SSL_clear),
[SSL\_SESSION\_free(3)](http://man.he.net/man3/SSL_SESSION_free)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
