# NAME

SSL\_CTX\_set\_mode, SSL\_set\_mode, SSL\_CTX\_get\_mode, SSL\_get\_mode - manipulate SSL engine mode

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_CTX_set_mode(SSL_CTX *ctx, long mode);
    long SSL_set_mode(SSL *ssl, long mode);

    long SSL_CTX_get_mode(SSL_CTX *ctx);
    long SSL_get_mode(SSL *ssl);

# DESCRIPTION

SSL\_CTX\_set\_mode() adds the mode set via bitmask in **mode** to **ctx**.
Options already set before are not cleared.

SSL\_set\_mode() adds the mode set via bitmask in **mode** to **ssl**.
Options already set before are not cleared.

SSL\_CTX\_get\_mode() returns the mode set for **ctx**.

SSL\_get\_mode() returns the mode set for **ssl**.

# NOTES

The following mode changes are available:

- SSL\_MODE\_ENABLE\_PARTIAL\_WRITE

    Allow SSL\_write(..., n) to return r with 0 < r < n (i.e. report success
    when just a single record has been written). When not set (the default),
    SSL\_write() will only report success once the complete chunk was written.
    Once SSL\_write() returns with r, r bytes have been successfully written
    and the next call to SSL\_write() must only send the n-r bytes left,
    imitating the behaviour of write().

- SSL\_MODE\_ACCEPT\_MOVING\_WRITE\_BUFFER

    Make it possible to retry SSL\_write() with changed buffer location
    (the buffer contents must stay the same). This is not the default to avoid
    the misconception that non-blocking SSL\_write() behaves like
    non-blocking write().

- SSL\_MODE\_AUTO\_RETRY

    Never bother the application with retries if the transport is blocking.
    If a renegotiation take place during normal operation, a
    [SSL\_read(3)](http://man.he.net/man3/SSL_read) or [SSL\_write(3)](http://man.he.net/man3/SSL_write) would return
    with -1 and indicate the need to retry with SSL\_ERROR\_WANT\_READ.
    In a non-blocking environment applications must be prepared to handle
    incomplete read/write operations.
    In a blocking environment, applications are not always prepared to
    deal with read/write operations returning without success report. The
    flag SSL\_MODE\_AUTO\_RETRY will cause read/write operations to only
    return after the handshake and successful completion.

- SSL\_MODE\_RELEASE\_BUFFERS

    When we no longer need a read buffer or a write buffer for a given SSL,
    then release the memory we were using to hold it.
    Using this flag can
    save around 34k per idle SSL connection.
    This flag has no effect on SSL v2 connections, or on DTLS connections.

- SSL\_MODE\_SEND\_FALLBACK\_SCSV

    Send TLS\_FALLBACK\_SCSV in the ClientHello.
    To be set only by applications that reconnect with a downgraded protocol
    version; see draft-ietf-tls-downgrade-scsv-00 for details.

    DO NOT ENABLE THIS if your application attempts a normal handshake.
    Only use this in explicit fallback retries, following the guidance
    in draft-ietf-tls-downgrade-scsv-00.

- SSL\_MODE\_ASYNC

    Enable asynchronous processing. TLS I/O operations may indicate a retry with
    SSL\_ERROR\_WANT\_ASYNC with this mode set if an asynchronous capable engine is
    used to perform cryptographic operations. See [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error).

# RETURN VALUES

SSL\_CTX\_set\_mode() and SSL\_set\_mode() return the new mode bitmask
after adding **mode**.

SSL\_CTX\_get\_mode() and SSL\_get\_mode() return the current bitmask.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_read(3)](http://man.he.net/man3/SSL_read), [SSL\_write(3)](http://man.he.net/man3/SSL_write), [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error)

# HISTORY

SSL\_MODE\_ASYNC was first added to OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
