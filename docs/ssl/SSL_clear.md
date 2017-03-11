# NAME

SSL\_clear - reset SSL object to allow another connection

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_clear(SSL *ssl);

# DESCRIPTION

Reset **ssl** to allow another connection. All settings (method, ciphers,
BIOs) are kept.

# NOTES

SSL\_clear is used to prepare an SSL object for a new connection. While all
settings are kept, a side effect is the handling of the current SSL session.
If a session is still **open**, it is considered bad and will be removed
from the session cache, as required by RFC2246. A session is considered open,
if [SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown) was not called for the connection
or at least [SSL\_set\_shutdown(3)](http://man.he.net/man3/SSL_set_shutdown) was used to
set the SSL\_SENT\_SHUTDOWN state.

If a session was closed cleanly, the session object will be kept and all
settings corresponding. This explicitly means, that e.g. the special method
used during the session will be kept for the next handshake. So if the
session was a TLSv1 session, a SSL client object will use a TLSv1 client
method for the next handshake and a SSL server object will use a TLSv1
server method, even if TLS\_\*\_methods were chosen on startup. This
will might lead to connection failures (see [SSL\_new(3)](http://man.he.net/man3/SSL_new))
for a description of the method's properties.

# WARNINGS

SSL\_clear() resets the SSL object to allow for another connection. The
reset operation however keeps several settings of the last sessions
(some of these settings were made automatically during the last
handshake). It only makes sense for a new connection with the exact
same peer that shares these settings, and may fail if that peer
changes its settings between connections. Use the sequence
[SSL\_get\_session(3)](http://man.he.net/man3/SSL_get_session);
[SSL\_new(3)](http://man.he.net/man3/SSL_new);
[SSL\_set\_session(3)](http://man.he.net/man3/SSL_set_session);
[SSL\_free(3)](http://man.he.net/man3/SSL_free)
instead to avoid such failures
(or simply [SSL\_free(3)](http://man.he.net/man3/SSL_free); [SSL\_new(3)](http://man.he.net/man3/SSL_new)
if session reuse is not desired).

# RETURN VALUES

The following return values can occur:

- 0

    The SSL\_clear() operation could not be performed. Check the error stack to
    find out the reason.

- 1

    The SSL\_clear() operation was successful.

[SSL\_new(3)](http://man.he.net/man3/SSL_new), [SSL\_free(3)](http://man.he.net/man3/SSL_free),
[SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown), [SSL\_set\_shutdown(3)](http://man.he.net/man3/SSL_set_shutdown),
[SSL\_CTX\_set\_options(3)](http://man.he.net/man3/SSL_CTX_set_options), [ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_set\_client\_cert\_cb(3)](http://man.he.net/man3/SSL_CTX_set_client_cert_cb)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
