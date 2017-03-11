# NAME

BIO\_s\_socket, BIO\_new\_socket - socket BIO

# SYNOPSIS

    #include <openssl/bio.h>

    const BIO_METHOD *BIO_s_socket(void);

    BIO *BIO_new_socket(int sock, int close_flag);

# DESCRIPTION

BIO\_s\_socket() returns the socket BIO method. This is a wrapper
round the platform's socket routines.

BIO\_read() and BIO\_write() read or write the underlying socket.
BIO\_puts() is supported but BIO\_gets() is not.

If the close flag is set then the socket is shut down and closed
when the BIO is freed.

BIO\_new\_socket() returns a socket BIO using **sock** and **close\_flag**.

# NOTES

Socket BIOs also support any relevant functionality of file descriptor
BIOs.

The reason for having separate file descriptor and socket BIOs is that on some
platforms sockets are not file descriptors and use distinct I/O routines,
Windows is one such platform. Any code mixing the two will not work on
all platforms.

# RETURN VALUES

BIO\_s\_socket() returns the socket BIO method.

BIO\_new\_socket() returns the newly allocated BIO or NULL is an error
occurred.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
