# NAME

SSL\_CTX\_sess\_set\_cache\_size, SSL\_CTX\_sess\_get\_cache\_size - manipulate session cache size

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_CTX_sess_set_cache_size(SSL_CTX *ctx, long t);
    long SSL_CTX_sess_get_cache_size(SSL_CTX *ctx);

# DESCRIPTION

SSL\_CTX\_sess\_set\_cache\_size() sets the size of the internal session cache
of context **ctx** to **t**.
This value is a hint and not an absolute; see the notes below.

SSL\_CTX\_sess\_get\_cache\_size() returns the currently valid session cache size.

# NOTES

The internal session cache size is SSL\_SESSION\_CACHE\_MAX\_SIZE\_DEFAULT,
currently 1024\*20, so that up to 20000 sessions can be held. This size
can be modified using the SSL\_CTX\_sess\_set\_cache\_size() call. A special
case is the size 0, which is used for unlimited size.

If adding the session makes the cache exceed its size, then unused
sessions are dropped from the end of the cache.
Cache space may also be reclaimed by calling
[SSL\_CTX\_flush\_sessions(3)](http://man.he.net/man3/SSL_CTX_flush_sessions) to remove
expired sessions.

If the size of the session cache is reduced and more sessions are already
in the session cache, old session will be removed at the next time a
session shall be added. This removal is not synchronized with the
expiration of sessions.

# RETURN VALUES

SSL\_CTX\_sess\_set\_cache\_size() returns the previously valid size.

SSL\_CTX\_sess\_get\_cache\_size() returns the currently valid size.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode),
[SSL\_CTX\_sess\_number(3)](http://man.he.net/man3/SSL_CTX_sess_number),
[SSL\_CTX\_flush\_sessions(3)](http://man.he.net/man3/SSL_CTX_flush_sessions)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
