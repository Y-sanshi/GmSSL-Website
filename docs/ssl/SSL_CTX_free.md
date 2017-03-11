# NAME

SSL\_CTX\_free - free an allocated SSL\_CTX object

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_CTX_free(SSL_CTX *ctx);

# DESCRIPTION

SSL\_CTX\_free() decrements the reference count of **ctx**, and removes the
SSL\_CTX object pointed to by **ctx** and frees up the allocated memory if the reference count has reached 0.

It also calls the free()ing procedures for indirectly affected items, if
applicable: the session cache, the list of ciphers, the list of Client CAs,
the certificates and keys.

If **ctx** is NULL nothing is done.

# WARNINGS

If a session-remove callback is set (SSL\_CTX\_sess\_set\_remove\_cb()), this
callback will be called for each session being freed from **ctx**'s
session cache. This implies, that all corresponding sessions from an
external session cache are removed as well. If this is not desired, the user
should explicitly unset the callback by calling
SSL\_CTX\_sess\_set\_remove\_cb(**ctx**, NULL) prior to calling SSL\_CTX\_free().

# RETURN VALUES

SSL\_CTX\_free() does not provide diagnostic information.

# SEE ALSO

[SSL\_CTX\_new(3)](http://man.he.net/man3/SSL_CTX_new), [ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_sess\_set\_get\_cb(3)](http://man.he.net/man3/SSL_CTX_sess_set_get_cb)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
