# NAME

BIO\_socket, BIO\_connect, BIO\_listen, BIO\_accept\_ex, BIO\_closesocket - BIO
socket communication setup routines

# SYNOPSIS

    #include <openssl/bio.h>

    int BIO_socket(int domain, int socktype, int protocol, int options);
    int BIO_connect(int sock, const BIO_ADDR *addr, int options);
    int BIO_listen(int sock, const BIO_ADDR *addr, int options);
    int BIO_accept_ex(int accept_sock, BIO_ADDR *peer, int options);
    int BIO_closesocket(int sock);

# DESCRIPTION

BIO\_socket() creates a socket in the domain **domain**, of type
**socktype** and **protocol**.  Socket **options** are currently unused,
but is present for future use.

BIO\_connect() connects **sock** to the address and service given by
**addr**.  Connection **options** may be zero or any combination of
**BIO\_SOCK\_KEEPALIVE**, **BIO\_SOCK\_NONBLOCK** and **BIO\_SOCK\_NODELAY**.
The flags are described in ["FLAGS"](#flags) below.

BIO\_listen() has **sock** start listening on the address and service
given by **addr**.  Connection **options** may be zero or any
combination of **BIO\_SOCK\_KEEPALIVE**, **BIO\_SOCK\_NONBLOCK**,
**BIO\_SOCK\_NODELAY**, **BIO\_SOCK\_REUSEADDR** and **BIO\_SOCK\_V6\_ONLY**.
The flags are described in ["FLAGS"](#flags) below.

BIO\_accept\_ex() waits for an incoming connections on the given
socket **accept\_sock**.  When it gets a connection, the address and
port of the peer gets stored in **peer** if that one is non-NULL.
Accept **options** may be zero or **BIO\_SOCK\_NONBLOCK**, and is applied
on the accepted socket.  The flags are described in ["FLAGS"](#flags) below.

BIO\_closesocket() closes **sock**.

# FLAGS

- BIO\_SOCK\_KEEPALIVE

    Enables regular sending of keep-alive messages.

- BIO\_SOCK\_NONBLOCK

    Sets the socket to non-blocking mode.

- BIO\_SOCK\_NODELAY

    Corresponds to **TCP\_NODELAY**, and disables the Nagle algorithm.  With
    this set, any data will be sent as soon as possible instead of being
    buffered until there's enough for the socket to send out in one go.

- BIO\_SOCK\_REUSEADDR

    Try to reuse the address and port combination for a recently closed
    port.

- BIO\_SOCK\_V6\_ONLY

    When creating an IPv6 socket, make it only listen for IPv6 addresses
    and not IPv4 addresses mapped to IPv6.

These flags are bit flags, so they are to be combined with the
`|` operator, for example:

    BIO_connect(sock, addr, BIO_SOCK_KEEPALIVE | BIO_SOCK_NONBLOCK);

# RETURN VALUES

BIO\_socket() returns the socket number on success or **INVALID\_SOCKET**
(-1) on error.  When an error has occurred, the OpenSSL error stack
will hold the error data and errno has the system error.

BIO\_connect() and BIO\_listen() return 1 on success or 0 on error.
When an error has occurred, the OpenSSL error stack will hold the error
data and errno has the system error.

BIO\_accept\_ex() returns the accepted socket on success or
**INVALID\_SOCKET** (-1) on error.  When an error has occurred, the
OpenSSL error stack will hold the error data and errno has the system
error.

# HISTORY

BIO\_gethostname(), BIO\_get\_port(), BIO\_get\_host\_ip(),
BIO\_get\_accept\_socket() and BIO\_accept() are deprecated since OpenSSL
1.1.  Use the functions described above instead.

# SEE ALSO

[BIO\_ADDR(3)](http://man.he.net/man3/BIO_ADDR)

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
