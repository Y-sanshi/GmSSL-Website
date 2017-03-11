# NAME

SSL\_get\_error - obtain result code for TLS/SSL I/O operation

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_get_error(const SSL *ssl, int ret);

# DESCRIPTION

SSL\_get\_error() returns a result code (suitable for the C "switch"
statement) for a preceding call to SSL\_connect(), SSL\_accept(), SSL\_do\_handshake(),
SSL\_read(), SSL\_peek(), or SSL\_write() on **ssl**.  The value returned by
that TLS/SSL I/O function must be passed to SSL\_get\_error() in parameter
**ret**.

In addition to **ssl** and **ret**, SSL\_get\_error() inspects the
current thread's OpenSSL error queue.  Thus, SSL\_get\_error() must be
used in the same thread that performed the TLS/SSL I/O operation, and no
other OpenSSL function calls should appear in between.  The current
thread's error queue must be empty before the TLS/SSL I/O operation is
attempted, or SSL\_get\_error() will not work reliably.

# RETURN VALUES

The following return values can currently occur:

- SSL\_ERROR\_NONE

    The TLS/SSL I/O operation completed.  This result code is returned
    if and only if **ret > 0**.

- SSL\_ERROR\_ZERO\_RETURN

    The TLS/SSL connection has been closed.
    If the protocol version is SSL 3.0 or higher, this result code is returned only
    if a closure alert has occurred in the protocol, i.e. if the connection has been
    closed cleanly.
    Note that in this case **SSL\_ERROR\_ZERO\_RETURN** does not necessarily
    indicate that the underlying transport has been closed.

- SSL\_ERROR\_WANT\_READ, SSL\_ERROR\_WANT\_WRITE

    The operation did not complete; the same TLS/SSL I/O function should be
    called again later.  If, by then, the underlying **BIO** has data
    available for reading (if the result code is **SSL\_ERROR\_WANT\_READ**)
    or allows writing data (**SSL\_ERROR\_WANT\_WRITE**), then some TLS/SSL
    protocol progress will take place, i.e. at least part of an TLS/SSL
    record will be read or written.  Note that the retry may again lead to
    a **SSL\_ERROR\_WANT\_READ** or **SSL\_ERROR\_WANT\_WRITE** condition.
    There is no fixed upper limit for the number of iterations that
    may be necessary until progress becomes visible at application
    protocol level.

    For socket **BIO**s (e.g. when SSL\_set\_fd() was used), select() or
    poll() on the underlying socket can be used to find out when the
    TLS/SSL I/O function should be retried.

    Caveat: Any TLS/SSL I/O function can lead to either of
    **SSL\_ERROR\_WANT\_READ** and **SSL\_ERROR\_WANT\_WRITE**.  In particular,
    SSL\_read() or SSL\_peek() may want to write data and SSL\_write() may want
    to read data.  This is mainly because TLS/SSL handshakes may occur at any
    time during the protocol (initiated by either the client or the server);
    SSL\_read(), SSL\_peek(), and SSL\_write() will handle any pending handshakes.

- SSL\_ERROR\_WANT\_CONNECT, SSL\_ERROR\_WANT\_ACCEPT

    The operation did not complete; the same TLS/SSL I/O function should be
    called again later. The underlying BIO was not connected yet to the peer
    and the call would block in connect()/accept(). The SSL function should be
    called again when the connection is established. These messages can only
    appear with a BIO\_s\_connect() or BIO\_s\_accept() BIO, respectively.
    In order to find out, when the connection has been successfully established,
    on many platforms select() or poll() for writing on the socket file descriptor
    can be used.

- SSL\_ERROR\_WANT\_X509\_LOOKUP

    The operation did not complete because an application callback set by
    SSL\_CTX\_set\_client\_cert\_cb() has asked to be called again.
    The TLS/SSL I/O function should be called again later.
    Details depend on the application.

- SSL\_ERROR\_WANT\_ASYNC

    The operation did not complete because an asynchronous engine is still
    processing data. This will only occur if the mode has been set to SSL\_MODE\_ASYNC
    using [SSL\_CTX\_set\_mode(3)](http://man.he.net/man3/SSL_CTX_set_mode) or [SSL\_set\_mode(3)](http://man.he.net/man3/SSL_set_mode) and an asynchronous capable
    engine is being used. An application can determine whether the engine has
    completed its processing using select() or poll() on the asynchronous wait file
    descriptor. This file descriptor is available by calling
    [SSL\_get\_all\_async\_fds(3)](http://man.he.net/man3/SSL_get_all_async_fds) or [SSL\_get\_changed\_async\_fds(3)](http://man.he.net/man3/SSL_get_changed_async_fds). The TLS/SSL I/O
    function should be called again later. The function **must** be called from the
    same thread that the original call was made from.

- SSL\_ERROR\_WANT\_ASYNC\_JOB

    The asynchronous job could not be started because there were no async jobs
    available in the pool (see ASYNC\_init\_thread(3)). This will only occur if the
    mode has been set to SSL\_MODE\_ASYNC using [SSL\_CTX\_set\_mode(3)](http://man.he.net/man3/SSL_CTX_set_mode) or
    [SSL\_set\_mode(3)](http://man.he.net/man3/SSL_set_mode) and a maximum limit has been set on the async job pool
    through a call to [ASYNC\_init\_thread(3)](http://man.he.net/man3/ASYNC_init_thread). The application should retry the
    operation after a currently executing asynchronous operation for the current
    thread has completed.

- SSL\_ERROR\_SYSCALL

    Some non-recoverable I/O error occurred.
    The OpenSSL error queue may contain more information on the error.
    For socket I/O on Unix systems, consult **errno** for details.

- SSL\_ERROR\_SSL

    A failure in the SSL library occurred, usually a protocol error.  The
    OpenSSL error queue contains more information on the error.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [err(3)](http://man.he.net/man3/err)

# HISTORY

SSL\_ERROR\_WANT\_ASYNC was added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
