# NAME

SSL\_CTX\_set\_session\_cache\_mode, SSL\_CTX\_get\_session\_cache\_mode - enable/disable session caching

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_CTX_set_session_cache_mode(SSL_CTX ctx, long mode);
    long SSL_CTX_get_session_cache_mode(SSL_CTX ctx);

# DESCRIPTION

SSL\_CTX\_set\_session\_cache\_mode() enables/disables session caching
by setting the operational mode for **ctx** to &lt;mode>.

SSL\_CTX\_get\_session\_cache\_mode() returns the currently used cache mode.

# NOTES

The OpenSSL library can store/retrieve SSL/TLS sessions for later reuse.
The sessions can be held in memory for each **ctx**, if more than one
SSL\_CTX object is being maintained, the sessions are unique for each SSL\_CTX
object.

In order to reuse a session, a client must send the session's id to the
server. It can only send exactly one id.  The server then either
agrees to reuse the session or it starts a full handshake (to create a new
session).

A server will look up the session in its internal session storage. If the
session is not found in internal storage or lookups for the internal storage
have been deactivated (SSL\_SESS\_CACHE\_NO\_INTERNAL\_LOOKUP), the server will try
the external storage if available.

Since a client may try to reuse a session intended for use in a different
context, the session id context must be set by the server (see
[SSL\_CTX\_set\_session\_id\_context(3)](http://man.he.net/man3/SSL_CTX_set_session_id_context)).

The following session cache modes and modifiers are available:

- SSL\_SESS\_CACHE\_OFF

    No session caching for client or server takes place.

- SSL\_SESS\_CACHE\_CLIENT

    Client sessions are added to the session cache. As there is no reliable way
    for the OpenSSL library to know whether a session should be reused or which
    session to choose (due to the abstract BIO layer the SSL engine does not
    have details about the connection), the application must select the session
    to be reused by using the [SSL\_set\_session(3)](http://man.he.net/man3/SSL_set_session)
    function. This option is not activated by default.

- SSL\_SESS\_CACHE\_SERVER

    Server sessions are added to the session cache. When a client proposes a
    session to be reused, the server looks for the corresponding session in (first)
    the internal session cache (unless SSL\_SESS\_CACHE\_NO\_INTERNAL\_LOOKUP is set),
    then (second) in the external cache if available. If the session is found, the
    server will try to reuse the session.  This is the default.

- SSL\_SESS\_CACHE\_BOTH

    Enable both SSL\_SESS\_CACHE\_CLIENT and SSL\_SESS\_CACHE\_SERVER at the same time.

- SSL\_SESS\_CACHE\_NO\_AUTO\_CLEAR

    Normally the session cache is checked for expired sessions every
    255 connections using the
    [SSL\_CTX\_flush\_sessions(3)](http://man.he.net/man3/SSL_CTX_flush_sessions) function. Since
    this may lead to a delay which cannot be controlled, the automatic
    flushing may be disabled and
    [SSL\_CTX\_flush\_sessions(3)](http://man.he.net/man3/SSL_CTX_flush_sessions) can be called
    explicitly by the application.

- SSL\_SESS\_CACHE\_NO\_INTERNAL\_LOOKUP

    By setting this flag, session-resume operations in an SSL/TLS server will not
    automatically look up sessions in the internal cache, even if sessions are
    automatically stored there. If external session caching callbacks are in use,
    this flag guarantees that all lookups are directed to the external cache.
    As automatic lookup only applies for SSL/TLS servers, the flag has no effect on
    clients.

- SSL\_SESS\_CACHE\_NO\_INTERNAL\_STORE

    Depending on the presence of SSL\_SESS\_CACHE\_CLIENT and/or SSL\_SESS\_CACHE\_SERVER,
    sessions negotiated in an SSL/TLS handshake may be cached for possible reuse.
    Normally a new session is added to the internal cache as well as any external
    session caching (callback) that is configured for the SSL\_CTX. This flag will
    prevent sessions being stored in the internal cache (though the application can
    add them manually using [SSL\_CTX\_add\_session(3)](http://man.he.net/man3/SSL_CTX_add_session)). Note:
    in any SSL/TLS servers where external caching is configured, any successful
    session lookups in the external cache (ie. for session-resume requests) would
    normally be copied into the local cache before processing continues - this flag
    prevents these additions to the internal cache as well.

- SSL\_SESS\_CACHE\_NO\_INTERNAL

    Enable both SSL\_SESS\_CACHE\_NO\_INTERNAL\_LOOKUP and
    SSL\_SESS\_CACHE\_NO\_INTERNAL\_STORE at the same time.

The default mode is SSL\_SESS\_CACHE\_SERVER.

# RETURN VALUES

SSL\_CTX\_set\_session\_cache\_mode() returns the previously set cache mode.

SSL\_CTX\_get\_session\_cache\_mode() returns the currently set cache mode.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_set\_session(3)](http://man.he.net/man3/SSL_set_session),
[SSL\_session\_reused(3)](http://man.he.net/man3/SSL_session_reused),
[SSL\_CTX\_add\_session(3)](http://man.he.net/man3/SSL_CTX_add_session),
[SSL\_CTX\_sess\_number(3)](http://man.he.net/man3/SSL_CTX_sess_number),
[SSL\_CTX\_sess\_set\_cache\_size(3)](http://man.he.net/man3/SSL_CTX_sess_set_cache_size),
[SSL\_CTX\_sess\_set\_get\_cb(3)](http://man.he.net/man3/SSL_CTX_sess_set_get_cb),
[SSL\_CTX\_set\_session\_id\_context(3)](http://man.he.net/man3/SSL_CTX_set_session_id_context),
[SSL\_CTX\_set\_timeout(3)](http://man.he.net/man3/SSL_CTX_set_timeout),
[SSL\_CTX\_flush\_sessions(3)](http://man.he.net/man3/SSL_CTX_flush_sessions)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
