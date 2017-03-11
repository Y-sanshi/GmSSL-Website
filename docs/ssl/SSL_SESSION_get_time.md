# NAME

SSL\_SESSION\_get\_time, SSL\_SESSION\_set\_time, SSL\_SESSION\_get\_timeout,
SSL\_SESSION\_set\_timeout
SSL\_get\_time, SSL\_set\_time, SSL\_get\_timeout, SSL\_set\_timeout,
\- retrieve and manipulate session time and timeout settings

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_SESSION_get_time(const SSL_SESSION *s);
    long SSL_SESSION_set_time(SSL_SESSION *s, long tm);
    long SSL_SESSION_get_timeout(const SSL_SESSION *s);
    long SSL_SESSION_set_timeout(SSL_SESSION *s, long tm);

    long SSL_get_time(const SSL_SESSION *s);
    long SSL_set_time(SSL_SESSION *s, long tm);
    long SSL_get_timeout(const SSL_SESSION *s);
    long SSL_set_timeout(SSL_SESSION *s, long tm);

# DESCRIPTION

SSL\_SESSION\_get\_time() returns the time at which the session **s** was
established. The time is given in seconds since the Epoch and therefore
compatible to the time delivered by the time() call.

SSL\_SESSION\_set\_time() replaces the creation time of the session **s** with
the chosen value **tm**.

SSL\_SESSION\_get\_timeout() returns the timeout value set for session **s**
in seconds.

SSL\_SESSION\_set\_timeout() sets the timeout value for session **s** in seconds
to **tm**.

The SSL\_get\_time(), SSL\_set\_time(), SSL\_get\_timeout(), and SSL\_set\_timeout()
functions are synonyms for the SSL\_SESSION\_\*() counterparts.

# NOTES

Sessions are expired by examining the creation time and the timeout value.
Both are set at creation time of the session to the actual time and the
default timeout value at creation, respectively, as set by
[SSL\_CTX\_set\_timeout(3)](http://man.he.net/man3/SSL_CTX_set_timeout).
Using these functions it is possible to extend or shorten the lifetime
of the session.

# RETURN VALUES

SSL\_SESSION\_get\_time() and SSL\_SESSION\_get\_timeout() return the currently
valid values.

SSL\_SESSION\_set\_time() and SSL\_SESSION\_set\_timeout() return 1 on success.

If any of the function is passed the NULL pointer for the session **s**,
0 is returned.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_set\_timeout(3)](http://man.he.net/man3/SSL_CTX_set_timeout),
[SSL\_get\_default\_timeout(3)](http://man.he.net/man3/SSL_get_default_timeout)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
