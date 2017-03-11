# NAME

SSL\_SESSION\_free - free an allocated SSL\_SESSION structure

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_SESSION_free(SSL_SESSION *session);

# DESCRIPTION

SSL\_SESSION\_free() decrements the reference count of **session** and removes
the **SSL\_SESSION** structure pointed to by **session** and frees up the allocated
memory, if the reference count has reached 0.
If **session** is NULL nothing is done.

# NOTES

SSL\_SESSION objects are allocated, when a TLS/SSL handshake operation
is successfully completed. Depending on the settings, see
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode),
the SSL\_SESSION objects are internally referenced by the SSL\_CTX and
linked into its session cache. SSL objects may be using the SSL\_SESSION object;
as a session may be reused, several SSL objects may be using one SSL\_SESSION
object at the same time. It is therefore crucial to keep the reference
count (usage information) correct and not delete a SSL\_SESSION object
that is still used, as this may lead to program failures due to
dangling pointers. These failures may also appear delayed, e.g.
when an SSL\_SESSION object was completely freed as the reference count
incorrectly became 0, but it is still referenced in the internal
session cache and the cache list is processed during a
[SSL\_CTX\_flush\_sessions(3)](http://man.he.net/man3/SSL_CTX_flush_sessions) operation.

SSL\_SESSION\_free() must only be called for SSL\_SESSION objects, for
which the reference count was explicitly incremented (e.g.
by calling SSL\_get1\_session(), see [SSL\_get\_session(3)](http://man.he.net/man3/SSL_get_session))
or when the SSL\_SESSION object was generated outside a TLS handshake
operation, e.g. by using [d2i\_SSL\_SESSION(3)](http://man.he.net/man3/d2i_SSL_SESSION).
It must not be called on other SSL\_SESSION objects, as this would cause
incorrect reference counts and therefore program failures.

# RETURN VALUES

SSL\_SESSION\_free() does not provide diagnostic information.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_get\_session(3)](http://man.he.net/man3/SSL_get_session),
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode),
[SSL\_CTX\_flush\_sessions(3)](http://man.he.net/man3/SSL_CTX_flush_sessions),
[d2i\_SSL\_SESSION(3)](http://man.he.net/man3/d2i_SSL_SESSION)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
