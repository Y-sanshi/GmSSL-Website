# NAME

BIO\_set\_conn\_address, BIO\_get\_conn\_address,
BIO\_s\_connect, BIO\_new\_connect, BIO\_set\_conn\_hostname, BIO\_set\_conn\_port,
BIO\_get\_conn\_hostname,
BIO\_get\_conn\_port,
BIO\_set\_nbio, BIO\_do\_connect - connect BIO

# SYNOPSIS

    #include <openssl/bio.h>

    const BIO_METHOD * BIO_s_connect(void);

    BIO *BIO_new_connect(char *name);

    long BIO_set_conn_hostname(BIO *b, char *name);
    long BIO_set_conn_port(BIO *b, char *port);
    long BIO_set_conn_address(BIO *b, BIO_ADDR *addr);
    const char *BIO_get_conn_hostname(BIO *b);
    const char *BIO_get_conn_port(BIO *b);
    const BIO_ADDR *BIO_get_conn_address(BIO *b);

    long BIO_set_nbio(BIO *b, long n);

    int BIO_do_connect(BIO *b);

# DESCRIPTION

BIO\_s\_connect() returns the connect BIO method. This is a wrapper
round the platform's TCP/IP socket connection routines.

Using connect BIOs, TCP/IP connections can be made and data
transferred using only BIO routines. In this way any platform
specific operations are hidden by the BIO abstraction.

Read and write operations on a connect BIO will perform I/O
on the underlying connection. If no connection is established
and the port and hostname (see below) is set up properly then
a connection is established first.

Connect BIOs support BIO\_puts() but not BIO\_gets().

If the close flag is set on a connect BIO then any active
connection is shutdown and the socket closed when the BIO
is freed.

Calling BIO\_reset() on a connect BIO will close any active
connection and reset the BIO into a state where it can connect
to the same host again.

BIO\_get\_fd() places the underlying socket in **c** if it is not NULL,
it also returns the socket . If **c** is not NULL it should be of
type (int \*).

BIO\_set\_conn\_hostname() uses the string **name** to set the hostname.
The hostname can be an IP address; if the address is an IPv6 one, it
must be enclosed with brackets. The hostname can also include the
port in the form hostname:port.

BIO\_set\_conn\_port() sets the port to **port**. **port** can be the
numerical form or a string such as "http". A string will be looked
up first using getservbyname() on the host platform but if that
fails a standard table of port names will be used. This internal
list is http, telnet, socks, https, ssl, ftp, and gopher.

BIO\_set\_conn\_address() sets the address and port information using
a BIO\_ADDR(3ssl).

BIO\_get\_conn\_hostname() returns the hostname of the connect BIO or
NULL if the BIO is initialized but no hostname is set.
This return value is an internal pointer which should not be modified.

BIO\_get\_conn\_port() returns the port as a string.
This return value is an internal pointer which should not be modified.

BIO\_get\_conn\_address() returns the address information as a BIO\_ADDR.
This return value is an internal pointer which should not be modified.

BIO\_set\_nbio() sets the non blocking I/O flag to **n**. If **n** is
zero then blocking I/O is set. If **n** is 1 then non blocking I/O
is set. Blocking I/O is the default. The call to BIO\_set\_nbio()
should be made before the connection is established because
non blocking I/O is set during the connect process.

BIO\_new\_connect() combines BIO\_new() and BIO\_set\_conn\_hostname() into
a single call: that is it creates a new connect BIO with **name**.

BIO\_do\_connect() attempts to connect the supplied BIO. It returns 1
if the connection was established successfully. A zero or negative
value is returned if the connection could not be established, the
call BIO\_should\_retry() should be used for non blocking connect BIOs
to determine if the call should be retried.

# NOTES

If blocking I/O is set then a non positive return value from any
I/O call is caused by an error condition, although a zero return
will normally mean that the connection was closed.

If the port name is supplied as part of the host name then this will
override any value set with BIO\_set\_conn\_port(). This may be undesirable
if the application does not wish to allow connection to arbitrary
ports. This can be avoided by checking for the presence of the ':'
character in the passed hostname and either indicating an error or
truncating the string at that point.

The values returned by BIO\_get\_conn\_hostname(), BIO\_get\_conn\_port(),
BIO\_get\_conn\_ip() and BIO\_get\_conn\_int\_port() are updated when a
connection attempt is made. Before any connection attempt the values
returned are those set by the application itself.

Applications do not have to call BIO\_do\_connect() but may wish to do
so to separate the connection process from other I/O processing.

If non blocking I/O is set then retries will be requested as appropriate.

It addition to BIO\_should\_read() and BIO\_should\_write() it is also
possible for BIO\_should\_io\_special() to be true during the initial
connection process with the reason BIO\_RR\_CONNECT. If this is returned
then this is an indication that a connection attempt would block,
the application should then take appropriate action to wait until
the underlying socket has connected and retry the call.

BIO\_set\_conn\_hostname(), BIO\_set\_conn\_port(), BIO\_set\_conn\_ip(),
BIO\_set\_conn\_int\_port(), BIO\_get\_conn\_hostname(), BIO\_get\_conn\_port(),
BIO\_get\_conn\_ip(), BIO\_get\_conn\_int\_port(), BIO\_set\_nbio() and
BIO\_do\_connect() are macros.

# RETURN VALUES

BIO\_s\_connect() returns the connect BIO method.

BIO\_get\_fd() returns the socket or -1 if the BIO has not
been initialized.

BIO\_set\_conn\_hostname(), BIO\_set\_conn\_port(), BIO\_set\_conn\_ip() and
BIO\_set\_conn\_int\_port() always return 1.

BIO\_get\_conn\_hostname() returns the connected hostname or NULL is
none was set.

BIO\_get\_conn\_port() returns a string representing the connected
port or NULL if not set.

BIO\_get\_conn\_ip() returns a pointer to the connected IP address in
binary form or all zeros if not set.

BIO\_get\_conn\_int\_port() returns the connected port or 0 if none was
set.

BIO\_set\_nbio() always returns 1.

BIO\_do\_connect() returns 1 if the connection was successfully
established and 0 or -1 if the connection failed.

# EXAMPLE

This is example connects to a webserver on the local host and attempts
to retrieve a page and copy the result to standard output.

    BIO *cbio, *out;
    int len;
    char tmpbuf[1024];

    cbio = BIO_new_connect("localhost:http");
    out = BIO_new_fp(stdout, BIO_NOCLOSE);
    if (BIO_do_connect(cbio) <= 0) {
        fprintf(stderr, "Error connecting to server\n");
        ERR_print_errors_fp(stderr);
        exit(1);
    }
    BIO_puts(cbio, "GET / HTTP/1.0\n\n");
    for ( ; ; ) {
        len = BIO_read(cbio, tmpbuf, 1024);
        if (len <= 0)
            break;
        BIO_write(out, tmpbuf, len);
    }
    BIO_free(cbio);
    BIO_free(out);

# SEE ALSO

[BIO\_ADDR(3)](http://man.he.net/man3/BIO_ADDR)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
