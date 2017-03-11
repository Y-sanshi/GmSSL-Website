# NAME

SSL\_session\_reused - query whether a reused session was negotiated during handshake

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_session_reused(SSL *ssl);

# DESCRIPTION

Query, whether a reused session was negotiated during the handshake.

# NOTES

During the negotiation, a client can propose to reuse a session. The server
then looks up the session in its cache. If both client and server agree
on the session, it will be reused and a flag is being set that can be
queried by the application.

# RETURN VALUES

The following return values can occur:

- 0

    A new session was negotiated.

- 1

    A session was reused.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_set\_session(3)](http://man.he.net/man3/SSL_set_session),
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
