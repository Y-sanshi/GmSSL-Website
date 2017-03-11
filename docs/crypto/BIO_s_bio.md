# NAME

BIO\_s\_bio, BIO\_make\_bio\_pair, BIO\_destroy\_bio\_pair, BIO\_shutdown\_wr,
BIO\_set\_write\_buf\_size, BIO\_get\_write\_buf\_size, BIO\_new\_bio\_pair,
BIO\_get\_write\_guarantee, BIO\_ctrl\_get\_write\_guarantee, BIO\_get\_read\_request,
BIO\_ctrl\_get\_read\_request, BIO\_ctrl\_reset\_read\_request - BIO pair BIO

# SYNOPSIS

    #include <openssl/bio.h>

    const BIO_METHOD *BIO_s_bio(void);

    int BIO_make_bio_pair(BIO *b1, BIO *b2);
    int BIO_destroy_bio_pair(BIO *b);
    int BIO_shutdown_wr(BIO *b);


    int BIO_set_write_buf_size(BIO *b, long size);
    size_t BIO_get_write_buf_size(BIO *b, long size);

    int BIO_new_bio_pair(BIO **bio1, size_t writebuf1, BIO **bio2, size_t writebuf2);

    int BIO_get_write_guarantee(BIO *b);
    size_t BIO_ctrl_get_write_guarantee(BIO *b);
    int BIO_get_read_request(BIO *b);
    size_t BIO_ctrl_get_read_request(BIO *b);
    int BIO_ctrl_reset_read_request(BIO *b);

# DESCRIPTION

BIO\_s\_bio() returns the method for a BIO pair. A BIO pair is a pair of source/sink
BIOs where data written to either half of the pair is buffered and can be read from
the other half. Both halves must usually by handled by the same application thread
since no locking is done on the internal data structures.

Since BIO chains typically end in a source/sink BIO it is possible to make this
one half of a BIO pair and have all the data processed by the chain under application
control.

One typical use of BIO pairs is to place TLS/SSL I/O under application control, this
can be used when the application wishes to use a non standard transport for
TLS/SSL or the normal socket routines are inappropriate.

Calls to BIO\_read() will read data from the buffer or request a retry if no
data is available.

Calls to BIO\_write() will place data in the buffer or request a retry if the
buffer is full.

The standard calls BIO\_ctrl\_pending() and BIO\_ctrl\_wpending() can be used to
determine the amount of pending data in the read or write buffer.

BIO\_reset() clears any data in the write buffer.

BIO\_make\_bio\_pair() joins two separate BIOs into a connected pair.

BIO\_destroy\_pair() destroys the association between two connected BIOs. Freeing
up any half of the pair will automatically destroy the association.

BIO\_shutdown\_wr() is used to close down a BIO **b**. After this call no further
writes on BIO **b** are allowed (they will return an error). Reads on the other
half of the pair will return any pending data or EOF when all pending data has
been read.

BIO\_set\_write\_buf\_size() sets the write buffer size of BIO **b** to **size**.
If the size is not initialized a default value is used. This is currently
17K, sufficient for a maximum size TLS record.

BIO\_get\_write\_buf\_size() returns the size of the write buffer.

BIO\_new\_bio\_pair() combines the calls to BIO\_new(), BIO\_make\_bio\_pair() and
BIO\_set\_write\_buf\_size() to create a connected pair of BIOs **bio1**, **bio2**
with write buffer sizes **writebuf1** and **writebuf2**. If either size is
zero then the default size is used.  BIO\_new\_bio\_pair() does not check whether
**bio1** or **bio2** do point to some other BIO, the values are overwritten,
BIO\_free() is not called.

BIO\_get\_write\_guarantee() and BIO\_ctrl\_get\_write\_guarantee() return the maximum
length of data that can be currently written to the BIO. Writes larger than this
value will return a value from BIO\_write() less than the amount requested or if the
buffer is full request a retry. BIO\_ctrl\_get\_write\_guarantee() is a function
whereas BIO\_get\_write\_guarantee() is a macro.

BIO\_get\_read\_request() and BIO\_ctrl\_get\_read\_request() return the
amount of data requested, or the buffer size if it is less, if the
last read attempt at the other half of the BIO pair failed due to an
empty buffer.  This can be used to determine how much data should be
written to the BIO so the next read will succeed: this is most useful
in TLS/SSL applications where the amount of data read is usually
meaningful rather than just a buffer size. After a successful read
this call will return zero.  It also will return zero once new data
has been written satisfying the read request or part of it.
Note that BIO\_get\_read\_request() never returns an amount larger
than that returned by BIO\_get\_write\_guarantee().

BIO\_ctrl\_reset\_read\_request() can also be used to reset the value returned by
BIO\_get\_read\_request() to zero.

# NOTES

Both halves of a BIO pair should be freed. That is even if one half is implicit
freed due to a BIO\_free\_all() or SSL\_free() call the other half needs to be freed.

When used in bidirectional applications (such as TLS/SSL) care should be taken to
flush any data in the write buffer. This can be done by calling BIO\_pending()
on the other half of the pair and, if any data is pending, reading it and sending
it to the underlying transport. This must be done before any normal processing
(such as calling select() ) due to a request and BIO\_should\_read() being true.

To see why this is important consider a case where a request is sent using
BIO\_write() and a response read with BIO\_read(), this can occur during an
TLS/SSL handshake for example. BIO\_write() will succeed and place data in the write
buffer. BIO\_read() will initially fail and BIO\_should\_read() will be true. If
the application then waits for data to be available on the underlying transport
before flushing the write buffer it will never succeed because the request was
never sent!

BIO\_eof() is true if no data is in the peer BIO and the peer BIO has been
shutdown.

BIO\_make\_bio\_pair(), BIO\_destroy\_bio\_pair(), BIO\_shutdown\_wr(),
BIO\_set\_write\_buf\_size(), BIO\_get\_write\_buf\_size(),
BIO\_get\_write\_guarantee(), and BIO\_get\_read\_request() are implemented
as macros.

# RETURN VALUES

BIO\_new\_bio\_pair() returns 1 on success, with the new BIOs available in
**bio1** and **bio2**, or 0 on failure, with NULL pointers stored into the
locations for **bio1** and **bio2**. Check the error stack for more information.

\[XXXXX: More return values need to be added here\]

# EXAMPLE

The BIO pair can be used to have full control over the network access of an
application. The application can call select() on the socket as required
without having to go through the SSL-interface.

    BIO *internal_bio, *network_bio;
    ...
    BIO_new_bio_pair(&internal_bio, 0, &network_bio, 0);
    SSL_set_bio(ssl, internal_bio, internal_bio);
    SSL_operations(); //e.g SSL_read and SSL_write
    ...

    application |   TLS-engine
       |        |
       +----------> SSL_operations()
                |     /\    ||
                |     ||    \/
                |   BIO-pair (internal_bio)
                |   BIO-pair (network_bio)
                |     ||     /\
                |     \/     ||
       +-----------< BIO_operations()
       |        |
       |        |
      socket

     ...
     SSL_free(ssl);                /* implicitly frees internal_bio */
     BIO_free(network_bio);
     ...

As the BIO pair will only buffer the data and never directly access the
connection, it behaves non-blocking and will return as soon as the write
buffer is full or the read buffer is drained. Then the application has to
flush the write buffer and/or fill the read buffer.

Use the BIO\_ctrl\_pending(), to find out whether data is buffered in the BIO
and must be transferred to the network. Use BIO\_ctrl\_get\_read\_request() to
find out, how many bytes must be written into the buffer before the
SSL\_operation() can successfully be continued.

# WARNING

As the data is buffered, SSL\_operation() may return with an ERROR\_SSL\_WANT\_READ
condition, but there is still data in the write buffer. An application must
not rely on the error value of SSL\_operation() but must assure that the
write buffer is always flushed first. Otherwise a deadlock may occur as
the peer might be waiting for the data before being able to continue.

# SEE ALSO

[SSL\_set\_bio(3)](http://man.he.net/man3/SSL_set_bio), [ssl(3)](http://man.he.net/man3/ssl), [bio(3)](http://man.he.net/man3/bio),
[BIO\_should\_retry(3)](http://man.he.net/man3/BIO_should_retry), [BIO\_read(3)](http://man.he.net/man3/BIO_read)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
