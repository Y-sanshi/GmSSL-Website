# NAME

SSL\_read - read bytes from a TLS/SSL connection

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_read(SSL *ssl, void *buf, int num);

# DESCRIPTION

SSL\_read() tries to read **num** bytes from the specified **ssl** into the
buffer **buf**.

# NOTES

If necessary, SSL\_read() will negotiate a TLS/SSL session, if
not already explicitly performed by [SSL\_connect(3)](http://man.he.net/man3/SSL_connect) or
[SSL\_accept(3)](http://man.he.net/man3/SSL_accept). If the
peer requests a re-negotiation, it will be performed transparently during
the SSL\_read() operation. The behaviour of SSL\_read() depends on the
underlying BIO.

For the transparent negotiation to succeed, the **ssl** must have been
initialized to client or server mode. This is being done by calling
[SSL\_set\_connect\_state(3)](http://man.he.net/man3/SSL_set_connect_state) or SSL\_set\_accept\_state()
before the first call to an SSL\_read() or [SSL\_write(3)](http://man.he.net/man3/SSL_write)
function.

SSL\_read() works based on the SSL/TLS records. The data are received in
records (with a maximum record size of 16kB for SSLv3/TLSv1). Only when a
record has been completely received, it can be processed (decryption and
check of integrity). Therefore data that was not retrieved at the last
call of SSL\_read() can still be buffered inside the SSL layer and will be
retrieved on the next call to SSL\_read(). If **num** is higher than the
number of bytes buffered, SSL\_read() will return with the bytes buffered.
If no more bytes are in the buffer, SSL\_read() will trigger the processing
of the next record. Only when the record has been received and processed
completely, SSL\_read() will return reporting success. At most the contents
of the record will be returned. As the size of an SSL/TLS record may exceed
the maximum packet size of the underlying transport (e.g. TCP), it may
be necessary to read several packets from the transport layer before the
record is complete and SSL\_read() can succeed.

If the underlying BIO is **blocking**, SSL\_read() will only return, once the
read operation has been finished or an error occurred, except when a
renegotiation take place, in which case a SSL\_ERROR\_WANT\_READ may occur.
This behaviour can be controlled with the SSL\_MODE\_AUTO\_RETRY flag of the
[SSL\_CTX\_set\_mode(3)](http://man.he.net/man3/SSL_CTX_set_mode) call.

If the underlying BIO is **non-blocking**, SSL\_read() will also return
when the underlying BIO could not satisfy the needs of SSL\_read()
to continue the operation. In this case a call to
[SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error) with the
return value of SSL\_read() will yield **SSL\_ERROR\_WANT\_READ** or
**SSL\_ERROR\_WANT\_WRITE**. As at any time a re-negotiation is possible, a
call to SSL\_read() can also cause write operations! The calling process
then must repeat the call after taking appropriate action to satisfy the
needs of SSL\_read(). The action depends on the underlying BIO. When using a
non-blocking socket, nothing is to be done, but select() can be used to check
for the required condition. When using a buffering BIO, like a BIO pair, data
must be written into or retrieved out of the BIO before being able to continue.

[SSL\_pending(3)](http://man.he.net/man3/SSL_pending) can be used to find out whether there
are buffered bytes available for immediate retrieval. In this case
SSL\_read() can be called without blocking or actually receiving new
data from the underlying socket.

# WARNING

When an SSL\_read() operation has to be repeated because of
**SSL\_ERROR\_WANT\_READ** or **SSL\_ERROR\_WANT\_WRITE**, it must be repeated
with the same arguments.

# RETURN VALUES

The following return values can occur:

- > 0

    The read operation was successful.
    The return value is the number of bytes actually read from the TLS/SSL
    connection.

- <= 0

    The read operation was not successful, because either the connection was closed,
    an error occurred or action must be taken by the calling process.
    Call [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error) with the return value **ret** to find out the reason.

    Old documentation indicated a difference between 0 and -1, and that -1 was
    retryable.
    You should instead call SSL\_get\_error() to find out if it's retryable.

# SEE ALSO

[SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error), [SSL\_write(3)](http://man.he.net/man3/SSL_write),
[SSL\_CTX\_set\_mode(3)](http://man.he.net/man3/SSL_CTX_set_mode), [SSL\_CTX\_new(3)](http://man.he.net/man3/SSL_CTX_new),
[SSL\_connect(3)](http://man.he.net/man3/SSL_connect), [SSL\_accept(3)](http://man.he.net/man3/SSL_accept)
[SSL\_set\_connect\_state(3)](http://man.he.net/man3/SSL_set_connect_state),
[SSL\_pending(3)](http://man.he.net/man3/SSL_pending),
[SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown), [SSL\_set\_shutdown(3)](http://man.he.net/man3/SSL_set_shutdown),
[ssl(3)](http://man.he.net/man3/ssl), [bio(3)](http://man.he.net/man3/bio)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
