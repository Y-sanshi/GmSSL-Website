# NAME

SSL\_CTX\_set\_timeout, SSL\_CTX\_get\_timeout - manipulate timeout values for session caching

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_CTX_set_timeout(SSL_CTX *ctx, long t);
    long SSL_CTX_get_timeout(SSL_CTX *ctx);

# DESCRIPTION

SSL\_CTX\_set\_timeout() sets the timeout for newly created sessions for
**ctx** to **t**. The timeout value **t** must be given in seconds.

SSL\_CTX\_get\_timeout() returns the currently set timeout value for **ctx**.

# NOTES

Whenever a new session is created, it is assigned a maximum lifetime. This
lifetime is specified by storing the creation time of the session and the
timeout value valid at this time. If the actual time is later than creation
time plus timeout, the session is not reused.

Due to this realization, all sessions behave according to the timeout value
valid at the time of the session negotiation. Changes of the timeout value
do not affect already established sessions.

The expiration time of a single session can be modified using the
[SSL\_SESSION\_get\_time(3)](http://man.he.net/man3/SSL_SESSION_get_time) family of functions.

Expired sessions are removed from the internal session cache, whenever
[SSL\_CTX\_flush\_sessions(3)](http://man.he.net/man3/SSL_CTX_flush_sessions) is called, either
directly by the application or automatically (see
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode))

The default value for session timeout is decided on a per protocol
basis, see [SSL\_get\_default\_timeout(3)](http://man.he.net/man3/SSL_get_default_timeout).
All currently supported protocols have the same default timeout value
of 300 seconds.

# RETURN VALUES

SSL\_CTX\_set\_timeout() returns the previously set timeout value.

SSL\_CTX\_get\_timeout() returns the currently set timeout value.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode),
[SSL\_SESSION\_get\_time(3)](http://man.he.net/man3/SSL_SESSION_get_time),
[SSL\_CTX\_flush\_sessions(3)](http://man.he.net/man3/SSL_CTX_flush_sessions),
[SSL\_get\_default\_timeout(3)](http://man.he.net/man3/SSL_get_default_timeout)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
