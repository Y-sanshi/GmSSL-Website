# NAME

SSL\_CTX\_sess\_number, SSL\_CTX\_sess\_connect, SSL\_CTX\_sess\_connect\_good, SSL\_CTX\_sess\_connect\_renegotiate, SSL\_CTX\_sess\_accept, SSL\_CTX\_sess\_accept\_good, SSL\_CTX\_sess\_accept\_renegotiate, SSL\_CTX\_sess\_hits, SSL\_CTX\_sess\_cb\_hits, SSL\_CTX\_sess\_misses, SSL\_CTX\_sess\_timeouts, SSL\_CTX\_sess\_cache\_full - obtain session cache statistics

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_CTX_sess_number(SSL_CTX *ctx);
    long SSL_CTX_sess_connect(SSL_CTX *ctx);
    long SSL_CTX_sess_connect_good(SSL_CTX *ctx);
    long SSL_CTX_sess_connect_renegotiate(SSL_CTX *ctx);
    long SSL_CTX_sess_accept(SSL_CTX *ctx);
    long SSL_CTX_sess_accept_good(SSL_CTX *ctx);
    long SSL_CTX_sess_accept_renegotiate(SSL_CTX *ctx);
    long SSL_CTX_sess_hits(SSL_CTX *ctx);
    long SSL_CTX_sess_cb_hits(SSL_CTX *ctx);
    long SSL_CTX_sess_misses(SSL_CTX *ctx);
    long SSL_CTX_sess_timeouts(SSL_CTX *ctx);
    long SSL_CTX_sess_cache_full(SSL_CTX *ctx);

# DESCRIPTION

SSL\_CTX\_sess\_number() returns the current number of sessions in the internal
session cache.

SSL\_CTX\_sess\_connect() returns the number of started SSL/TLS handshakes in
client mode.

SSL\_CTX\_sess\_connect\_good() returns the number of successfully established
SSL/TLS sessions in client mode.

SSL\_CTX\_sess\_connect\_renegotiate() returns the number of start renegotiations
in client mode.

SSL\_CTX\_sess\_accept() returns the number of started SSL/TLS handshakes in
server mode.

SSL\_CTX\_sess\_accept\_good() returns the number of successfully established
SSL/TLS sessions in server mode.

SSL\_CTX\_sess\_accept\_renegotiate() returns the number of start renegotiations
in server mode.

SSL\_CTX\_sess\_hits() returns the number of successfully reused sessions.
In client mode a session set with [SSL\_set\_session(3)](http://man.he.net/man3/SSL_set_session)
successfully reused is counted as a hit. In server mode a session successfully
retrieved from internal or external cache is counted as a hit.

SSL\_CTX\_sess\_cb\_hits() returns the number of successfully retrieved sessions
from the external session cache in server mode.

SSL\_CTX\_sess\_misses() returns the number of sessions proposed by clients
that were not found in the internal session cache in server mode.

SSL\_CTX\_sess\_timeouts() returns the number of sessions proposed by clients
and either found in the internal or external session cache in server mode,
 but that were invalid due to timeout. These sessions are not included in
the SSL\_CTX\_sess\_hits() count.

SSL\_CTX\_sess\_cache\_full() returns the number of sessions that were removed
because the maximum session cache size was exceeded.

# RETURN VALUES

The functions return the values indicated in the DESCRIPTION section.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_set\_session(3)](http://man.he.net/man3/SSL_set_session),
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode)
[SSL\_CTX\_sess\_set\_cache\_size(3)](http://man.he.net/man3/SSL_CTX_sess_set_cache_size)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
