# NAME

SSL\_get\_fd, SSL\_get\_rfd, SSL\_get\_wfd - get file descriptor linked to an SSL object

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_get_fd(const SSL *ssl);
    int SSL_get_rfd(const SSL *ssl);
    int SSL_get_wfd(const SSL *ssl);

# DESCRIPTION

SSL\_get\_fd() returns the file descriptor which is linked to **ssl**.
SSL\_get\_rfd() and SSL\_get\_wfd() return the file descriptors for the
read or the write channel, which can be different. If the read and the
write channel are different, SSL\_get\_fd() will return the file descriptor
of the read channel.

# RETURN VALUES

The following return values can occur:

- -1

    The operation failed, because the underlying BIO is not of the correct type
    (suitable for file descriptors).

- >=0

    The file descriptor linked to **ssl**.

# SEE ALSO

[SSL\_set\_fd(3)](http://man.he.net/man3/SSL_set_fd), [ssl(3)](http://man.he.net/man3/ssl) , [bio(3)](http://man.he.net/man3/bio)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
