# NAME

SSL\_write - write bytes to a TLS/SSL connection

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_write(SSL *ssl, const void *buf, int num);

# DESCRIPTION

SSL\_write() writes **num** bytes from the buffer **buf** into the specified
**ssl** connection.

# NOTES

If necessary, SSL\_write() will negotiate a TLS/SSL session, if
not already explicitly performed by [SSL\_connect(3)](http://man.he.net/man3/SSL_connect) or
[SSL\_accept(3)](http://man.he.net/man3/SSL_accept). If the
peer requests a re-negotiation, it will be performed transparently during
the SSL\_write() operation. The behaviour of SSL\_write() depends on the
underlying BIO.

For the transparent negotiation to succeed, the **ssl** must have been
initialized to client or server mode. This is being done by calling
[SSL\_set\_connect\_state(3)](http://man.he.net/man3/SSL_set_connect_state) or SSL\_set\_accept\_state()
before the first call to an [SSL\_read(3)](http://man.he.net/man3/SSL_read) or SSL\_write() function.

If the underlying BIO is **blocking**, SSL\_write() will only return, once the
write operation has been finished or an error occurred, except when a
renegotiation take place, in which case a SSL\_ERROR\_WANT\_READ may occur.
This behaviour can be controlled with the SSL\_MODE\_AUTO\_RETRY flag of the
[SSL\_CTX\_set\_mode(3)](http://man.he.net/man3/SSL_CTX_set_mode) call.

If the underlying BIO is **non-blocking**, SSL\_write() will also return,
when the underlying BIO could not satisfy the needs of SSL\_write()
to continue the operation. In this case a call to
[SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error) with the
return value of SSL\_write() will yield **SSL\_ERROR\_WANT\_READ** or
**SSL\_ERROR\_WANT\_WRITE**. As at any time a re-negotiation is possible, a
call to SSL\_write() can also cause read operations! The calling process
then must repeat the call after taking appropriate action to satisfy the
needs of SSL\_write(). The action depends on the underlying BIO. When using a
non-blocking socket, nothing is to be done, but select() can be used to check
for the required condition. When using a buffering BIO, like a BIO pair, data
must be written into or retrieved out of the BIO before being able to continue.

SSL\_write() will only return with success, when the complete contents
of **buf** of length **num** has been written. This default behaviour
can be changed with the SSL\_MODE\_ENABLE\_PARTIAL\_WRITE option of
[SSL\_CTX\_set\_mode(3)](http://man.he.net/man3/SSL_CTX_set_mode). When this flag is set,
SSL\_write() will also return with success, when a partial write has been
successfully completed. In this case the SSL\_write() operation is considered
completed. The bytes are sent and a new SSL\_write() operation with a new
buffer (with the already sent bytes removed) must be started.
A partial write is performed with the size of a message block, which is
16kB for SSLv3/TLSv1.

# WARNING

When an SSL\_write() operation has to be repeated because of
**SSL\_ERROR\_WANT\_READ** or **SSL\_ERROR\_WANT\_WRITE**, it must be repeated
with the same arguments.

When calling SSL\_write() with num=0 bytes to be sent the behaviour is
undefined.

# RETURN VALUES

The following return values can occur:

- > 0

    The write operation was successful, the return value is the number of
    bytes actually written to the TLS/SSL connection.

- <= 0

    The write operation was not successful, because either the connection was
    closed, an error occurred or action must be taken by the calling process.
    Call SSL\_get\_error() with the return value **ret** to find out the reason.

    Old documentation indicated a difference between 0 and -1, and that -1 was
    retryable.
    You should instead call SSL\_get\_error() to find out if it's retryable.

# SEE ALSO

[SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error), [SSL\_read(3)](http://man.he.net/man3/SSL_read),
[SSL\_CTX\_set\_mode(3)](http://man.he.net/man3/SSL_CTX_set_mode), [SSL\_CTX\_new(3)](http://man.he.net/man3/SSL_CTX_new),
[SSL\_connect(3)](http://man.he.net/man3/SSL_connect), [SSL\_accept(3)](http://man.he.net/man3/SSL_accept)
[SSL\_set\_connect\_state(3)](http://man.he.net/man3/SSL_set_connect_state),
[ssl(3)](http://man.he.net/man3/ssl), [bio(3)](http://man.he.net/man3/bio)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
