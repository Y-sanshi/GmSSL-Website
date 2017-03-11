# NAME

SSL\_free - free an allocated SSL structure

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_free(SSL *ssl);

# DESCRIPTION

SSL\_free() decrements the reference count of **ssl**, and removes the SSL
structure pointed to by **ssl** and frees up the allocated memory if the
reference count has reached 0.
If **ssl** is NULL nothing is done.

# NOTES

SSL\_free() also calls the free()ing procedures for indirectly affected items, if
applicable: the buffering BIO, the read and write BIOs,
cipher lists specially created for this **ssl**, the **SSL\_SESSION**.
Do not explicitly free these indirectly freed up items before or after
calling SSL\_free(), as trying to free things twice may lead to program
failure.

The ssl session has reference counts from two users: the SSL object, for
which the reference count is removed by SSL\_free() and the internal
session cache. If the session is considered bad, because
[SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown) was not called for the connection
and [SSL\_set\_shutdown(3)](http://man.he.net/man3/SSL_set_shutdown) was not used to set the
SSL\_SENT\_SHUTDOWN state, the session will also be removed
from the session cache as required by RFC2246.

# RETURN VALUES

SSL\_free() does not provide diagnostic information.

[SSL\_new(3)](http://man.he.net/man3/SSL_new), [SSL\_clear(3)](http://man.he.net/man3/SSL_clear),
[SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown), [SSL\_set\_shutdown(3)](http://man.he.net/man3/SSL_set_shutdown),
[ssl(3)](http://man.he.net/man3/ssl)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
