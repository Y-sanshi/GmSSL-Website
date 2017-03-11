# NAME

SSL\_CTX\_add\_session, SSL\_add\_session, SSL\_CTX\_remove\_session, SSL\_remove\_session - manipulate session cache

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CTX_add_session(SSL_CTX *ctx, SSL_SESSION *c);
    int SSL_add_session(SSL_CTX *ctx, SSL_SESSION *c);

    int SSL_CTX_remove_session(SSL_CTX *ctx, SSL_SESSION *c);
    int SSL_remove_session(SSL_CTX *ctx, SSL_SESSION *c);

# DESCRIPTION

SSL\_CTX\_add\_session() adds the session **c** to the context **ctx**. The
reference count for session **c** is incremented by 1. If a session with
the same session id already exists, the old session is removed by calling
[SSL\_SESSION\_free(3)](http://man.he.net/man3/SSL_SESSION_free).

SSL\_CTX\_remove\_session() removes the session **c** from the context **ctx**.
[SSL\_SESSION\_free(3)](http://man.he.net/man3/SSL_SESSION_free) is called once for **c**.

SSL\_add\_session() and SSL\_remove\_session() are synonyms for their
SSL\_CTX\_\*() counterparts.

# NOTES

When adding a new session to the internal session cache, it is examined
whether a session with the same session id already exists. In this case
it is assumed that both sessions are identical. If the same session is
stored in a different SSL\_SESSION object, The old session is
removed and replaced by the new session. If the session is actually
identical (the SSL\_SESSION object is identical), SSL\_CTX\_add\_session()
is a no-op, and the return value is 0.

If a server SSL\_CTX is configured with the SSL\_SESS\_CACHE\_NO\_INTERNAL\_STORE
flag then the internal cache will not be populated automatically by new
sessions negotiated by the SSL/TLS implementation, even though the internal
cache will be searched automatically for session-resume requests (the
latter can be suppressed by SSL\_SESS\_CACHE\_NO\_INTERNAL\_LOOKUP). So the
application can use SSL\_CTX\_add\_session() directly to have full control
over the sessions that can be resumed if desired.

# RETURN VALUES

The following values are returned by all functions:

- 0

        The operation failed. In case of the add operation, it was tried to add
        the same (identical) session twice. In case of the remove operation, the
        session was not found in the cache.

- 1

        The operation succeeded.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode),
[SSL\_SESSION\_free(3)](http://man.he.net/man3/SSL_SESSION_free)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
