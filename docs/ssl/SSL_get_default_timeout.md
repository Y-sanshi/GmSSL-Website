# NAME

SSL\_get\_default\_timeout - get default session timeout value

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_get_default_timeout(const SSL *ssl);

# DESCRIPTION

SSL\_get\_default\_timeout() returns the default timeout value assigned to
SSL\_SESSION objects negotiated for the protocol valid for **ssl**.

# NOTES

Whenever a new session is negotiated, it is assigned a timeout value,
after which it will not be accepted for session reuse. If the timeout
value was not explicitly set using
[SSL\_CTX\_set\_timeout(3)](http://man.he.net/man3/SSL_CTX_set_timeout), the hardcoded default
timeout for the protocol will be used.

SSL\_get\_default\_timeout() return this hardcoded value, which is 300 seconds
for all currently supported protocols.

# RETURN VALUES

See description.

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
