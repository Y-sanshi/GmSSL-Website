# NAME

SSL\_set\_session - set a TLS/SSL session to be used during TLS/SSL connect

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_set_session(SSL *ssl, SSL_SESSION *session);

# DESCRIPTION

SSL\_set\_session() sets **session** to be used when the TLS/SSL connection
is to be established. SSL\_set\_session() is only useful for TLS/SSL clients.
When the session is set, the reference count of **session** is incremented
by 1. If the session is not reused, the reference count is decremented
again during SSL\_connect(). Whether the session was reused can be queried
with the [SSL\_session\_reused(3)](http://man.he.net/man3/SSL_session_reused) call.

If there is already a session set inside **ssl** (because it was set with
SSL\_set\_session() before or because the same **ssl** was already used for
a connection), SSL\_SESSION\_free() will be called for that session. If that old
session is still **open**, it is considered bad and will be removed from the
session cache (if used). A session is considered open, if [SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown) was
not called for the connection (or at least [SSL\_set\_shutdown(3)](http://man.he.net/man3/SSL_set_shutdown) was used to
set the SSL\_SENT\_SHUTDOWN state).

# NOTES

SSL\_SESSION objects keep internal link information about the session cache
list, when being inserted into one SSL\_CTX object's session cache.
One SSL\_SESSION object, regardless of its reference count, must therefore
only be used with one SSL\_CTX object (and the SSL objects created
from this SSL\_CTX object).

# RETURN VALUES

The following return values can occur:

- 0

    The operation failed; check the error stack to find out the reason.

- 1

    The operation succeeded.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_SESSION\_free(3)](http://man.he.net/man3/SSL_SESSION_free),
[SSL\_get\_session(3)](http://man.he.net/man3/SSL_get_session),
[SSL\_session\_reused(3)](http://man.he.net/man3/SSL_session_reused),
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
