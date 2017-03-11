# NAME

SSL\_CTX\_sessions - access internal session cache

# SYNOPSIS

    #include <openssl/ssl.h>

    struct lhash_st *SSL_CTX_sessions(SSL_CTX *ctx);

# DESCRIPTION

SSL\_CTX\_sessions() returns a pointer to the lhash databases containing the
internal session cache for **ctx**.

# NOTES

The sessions in the internal session cache are kept in an
[lhash(3)](http://man.he.net/man3/lhash) type database. It is possible to directly
access this database e.g. for searching. In parallel, the sessions
form a linked list which is maintained separately from the
[lhash(3)](http://man.he.net/man3/lhash) operations, so that the database must not be
modified directly but by using the
[SSL\_CTX\_add\_session(3)](http://man.he.net/man3/SSL_CTX_add_session) family of functions.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [lhash(3)](http://man.he.net/man3/lhash),
[SSL\_CTX\_add\_session(3)](http://man.he.net/man3/SSL_CTX_add_session),
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
