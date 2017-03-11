# NAME

BIO\_s\_accept, BIO\_set\_accept\_name, BIO\_set\_accept\_port, BIO\_get\_accept\_name,
BIO\_get\_accept\_port, BIO\_new\_accept, BIO\_set\_nbio\_accept, BIO\_set\_accept\_bios,
BIO\_set\_bind\_mode, BIO\_get\_bind\_mode, BIO\_do\_accept - accept BIO

# SYNOPSIS

    #include <openssl/bio.h>

    const BIO_METHOD *BIO_s_accept(void);

    long BIO_set_accept_name(BIO *b, char *name);
    char *BIO_get_accept_name(BIO *b);

    long BIO_set_accept_port(BIO *b, char *port);
    char *BIO_get_accept_port(BIO *b);

    BIO *BIO_new_accept(char *host_port);

    long BIO_set_nbio_accept(BIO *b, int n);
    long BIO_set_accept_bios(BIO *b, char *bio);

    long BIO_set_bind_mode(BIO *b, long mode);
    long BIO_get_bind_mode(BIO *b);

    int BIO_do_accept(BIO *b);

# DESCRIPTION

BIO\_s\_accept() returns the accept BIO method. This is a wrapper
round the platform's TCP/IP socket accept routines.

Using accept BIOs, TCP/IP connections can be accepted and data
transferred using only BIO routines. In this way any platform
specific operations are hidden by the BIO abstraction.

Read and write operations on an accept BIO will perform I/O
on the underlying connection. If no connection is established
and the port (see below) is set up properly then the BIO
waits for an incoming connection.

Accept BIOs support BIO\_puts() but not BIO\_gets().

If the close flag is set on an accept BIO then any active
connection on that chain is shutdown and the socket closed when
the BIO is freed.

Calling BIO\_reset() on an accept BIO will close any active
connection and reset the BIO into a state where it awaits another
incoming connection.

BIO\_get\_fd() and BIO\_set\_fd() can be called to retrieve or set
the accept socket. See [BIO\_s\_fd(3)](http://man.he.net/man3/BIO_s_fd)

BIO\_set\_accept\_name() uses the string **name** to set the accept
name. The name is represented as a string of the form "host:port",
where "host" is the interface to use and "port" is the port.
The host can be "\*" or empty which is interpreted as meaning
any interface.  If the host is an IPv6 address, it has to be
enclosed in brackets, for example "\[::1\]:https".  "port" has the
same syntax as the port specified in BIO\_set\_conn\_port() for
connect BIOs, that is it can be a numerical port string or a
string to lookup using getservbyname() and a string table.

BIO\_set\_accept\_port() uses the string **port** to set the accept
port.  "port" has the same syntax as the port specified in
BIO\_set\_conn\_port() for connect BIOs, that is it can be a numerical
port string or a string to lookup using getservbyname() and a string
table.

BIO\_new\_accept() combines BIO\_new() and BIO\_set\_accept\_name() into
a single call: that is it creates a new accept BIO with port
**host\_port**.

BIO\_set\_nbio\_accept() sets the accept socket to blocking mode
(the default) if **n** is 0 or non blocking mode if **n** is 1.

BIO\_set\_accept\_bios() can be used to set a chain of BIOs which
will be duplicated and prepended to the chain when an incoming
connection is received. This is useful if, for example, a
buffering or SSL BIO is required for each connection. The
chain of BIOs must not be freed after this call, they will
be automatically freed when the accept BIO is freed.

BIO\_set\_bind\_mode() and BIO\_get\_bind\_mode() set and retrieve
the current bind mode. If **BIO\_BIND\_NORMAL** (the default) is set
then another socket cannot be bound to the same port. If
**BIO\_BIND\_REUSEADDR** is set then other sockets can bind to the
same port. If **BIO\_BIND\_REUSEADDR\_IF\_UNUSED** is set then and
attempt is first made to use BIO\_BIN\_NORMAL, if this fails
and the port is not in use then a second attempt is made
using **BIO\_BIND\_REUSEADDR**.

BIO\_do\_accept() serves two functions. When it is first
called, after the accept BIO has been setup, it will attempt
to create the accept socket and bind an address to it. Second
and subsequent calls to BIO\_do\_accept() will await an incoming
connection, or request a retry in non blocking mode.

# NOTES

When an accept BIO is at the end of a chain it will await an
incoming connection before processing I/O calls. When an accept
BIO is not at then end of a chain it passes I/O calls to the next
BIO in the chain.

When a connection is established a new socket BIO is created for
the connection and appended to the chain. That is the chain is now
accept->socket. This effectively means that attempting I/O on
an initial accept socket will await an incoming connection then
perform I/O on it.

If any additional BIOs have been set using BIO\_set\_accept\_bios()
then they are placed between the socket and the accept BIO,
that is the chain will be accept->otherbios->socket.

If a server wishes to process multiple connections (as is normally
the case) then the accept BIO must be made available for further
incoming connections. This can be done by waiting for a connection and
then calling:

    connection = BIO_pop(accept);

After this call **connection** will contain a BIO for the recently
established connection and **accept** will now be a single BIO
again which can be used to await further incoming connections.
If no further connections will be accepted the **accept** can
be freed using BIO\_free().

If only a single connection will be processed it is possible to
perform I/O using the accept BIO itself. This is often undesirable
however because the accept BIO will still accept additional incoming
connections. This can be resolved by using BIO\_pop() (see above)
and freeing up the accept BIO after the initial connection.

If the underlying accept socket is non-blocking and BIO\_do\_accept() is
called to await an incoming connection it is possible for
BIO\_should\_io\_special() with the reason BIO\_RR\_ACCEPT. If this happens
then it is an indication that an accept attempt would block: the application
should take appropriate action to wait until the underlying socket has
accepted a connection and retry the call.

BIO\_set\_accept\_name(), BIO\_get\_accept\_name(), BIO\_set\_accept\_port(),
BIO\_get\_accept\_port(), BIO\_set\_nbio\_accept(), BIO\_set\_accept\_bios(),
BIO\_set\_bind\_mode(), BIO\_get\_bind\_mode() and BIO\_do\_accept() are macros.

# RETURN VALUES

BIO\_do\_accept(),
BIO\_set\_accept\_name(), BIO\_set\_accept\_port(), BIO\_set\_nbio\_accept(),
BIO\_set\_accept\_bios(), and BIO\_set\_bind\_mode(), return 1 for success and 0 or
\-1 for failure.

BIO\_get\_accept\_name() returns the accept name or NULL on error.

BIO\_get\_accept\_port() returns the port as a string or NULL on error.

BIO\_get\_bind\_mode() returns the set of **BIO\_BIND** flags, or -1 on failure.

BIO\_new\_accept() returns a BIO or NULL on error.

# EXAMPLE

This example accepts two connections on port 4444, sends messages
down each and finally closes both down.

    BIO *abio, *cbio, *cbio2;

    /* First call to BIO_accept() sets up accept BIO */
    abio = BIO_new_accept("4444");
    if (BIO_do_accept(abio) <= 0) {
        fprintf(stderr, "Error setting up accept\n");
        ERR_print_errors_fp(stderr);
        exit(1);
    }

    /* Wait for incoming connection */
    if (BIO_do_accept(abio) <= 0) {
        fprintf(stderr, "Error accepting connection\n");
        ERR_print_errors_fp(stderr);
        exit(1);
    }
    fprintf(stderr, "Connection 1 established\n");

    /* Retrieve BIO for connection */
    cbio = BIO_pop(abio);
    BIO_puts(cbio, "Connection 1: Sending out Data on initial connection\n");
    fprintf(stderr, "Sent out data on connection 1\n");

    /* Wait for another connection */
    if (BIO_do_accept(abio) <= 0) {
        fprintf(stderr, "Error accepting connection\n");
        ERR_print_errors_fp(stderr);
        exit(1);
    }
    fprintf(stderr, "Connection 2 established\n");

    /* Close accept BIO to refuse further connections */
    cbio2 = BIO_pop(abio);
    BIO_free(abio);
    BIO_puts(cbio2, "Connection 2: Sending out Data on second\n");
    fprintf(stderr, "Sent out data on connection 2\n");

    BIO_puts(cbio, "Connection 1: Second connection established\n");

    /* Close the two established connections */
    BIO_free(cbio);
    BIO_free(cbio2);

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
