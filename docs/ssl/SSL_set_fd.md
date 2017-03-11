# NAME

SSL\_set\_fd, SSL\_set\_rfd, SSL\_set\_wfd - connect the SSL object with a file descriptor

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_set_fd(SSL *ssl, int fd);
    int SSL_set_rfd(SSL *ssl, int fd);
    int SSL_set_wfd(SSL *ssl, int fd);

# DESCRIPTION

SSL\_set\_fd() sets the file descriptor **fd** as the input/output facility
for the TLS/SSL (encrypted) side of **ssl**. **fd** will typically be the
socket file descriptor of a network connection.

When performing the operation, a **socket BIO** is automatically created to
interface between the **ssl** and **fd**. The BIO and hence the SSL engine
inherit the behaviour of **fd**. If **fd** is non-blocking, the **ssl** will
also have non-blocking behaviour.

If there was already a BIO connected to **ssl**, BIO\_free() will be called
(for both the reading and writing side, if different).

SSL\_set\_rfd() and SSL\_set\_wfd() perform the respective action, but only
for the read channel or the write channel, which can be set independently.

# RETURN VALUES

The following return values can occur:

- 0

    The operation failed. Check the error stack to find out why.

- 1

    The operation succeeded.

# SEE ALSO

[SSL\_get\_fd(3)](http://man.he.net/man3/SSL_get_fd), [SSL\_set\_bio(3)](http://man.he.net/man3/SSL_set_bio),
[SSL\_connect(3)](http://man.he.net/man3/SSL_connect), [SSL\_accept(3)](http://man.he.net/man3/SSL_accept),
[SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown), [ssl(3)](http://man.he.net/man3/ssl) , [bio(3)](http://man.he.net/man3/bio)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
