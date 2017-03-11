# NAME

SSL\_CTX\_flush\_sessions, SSL\_flush\_sessions - remove expired sessions

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_CTX_flush_sessions(SSL_CTX *ctx, long tm);
    void SSL_flush_sessions(SSL_CTX *ctx, long tm);

# DESCRIPTION

SSL\_CTX\_flush\_sessions() causes a run through the session cache of
**ctx** to remove sessions expired at time **tm**.

SSL\_flush\_sessions() is a synonym for SSL\_CTX\_flush\_sessions().

# NOTES

If enabled, the internal session cache will collect all sessions established
up to the specified maximum number (see SSL\_CTX\_sess\_set\_cache\_size()).
As sessions will not be reused ones they are expired, they should be
removed from the cache to save resources. This can either be done
 automatically whenever 255 new sessions were established (see
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode))
or manually by calling SSL\_CTX\_flush\_sessions().

The parameter **tm** specifies the time which should be used for the
expiration test, in most cases the actual time given by time(0)
will be used.

SSL\_CTX\_flush\_sessions() will only check sessions stored in the internal
cache. When a session is found and removed, the remove\_session\_cb is however
called to synchronize with the external cache (see
[SSL\_CTX\_sess\_set\_get\_cb(3)](http://man.he.net/man3/SSL_CTX_sess_set_get_cb)).

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode),
[SSL\_CTX\_set\_timeout(3)](http://man.he.net/man3/SSL_CTX_set_timeout),
[SSL\_CTX\_sess\_set\_get\_cb(3)](http://man.he.net/man3/SSL_CTX_sess_set_get_cb)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
