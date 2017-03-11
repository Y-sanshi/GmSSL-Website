# NAME

SSL\_connect - initiate the TLS/SSL handshake with an TLS/SSL server

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_connect(SSL *ssl);

# DESCRIPTION

SSL\_connect() initiates the TLS/SSL handshake with a server. The communication
channel must already have been set and assigned to the **ssl** by setting an
underlying **BIO**.

# NOTES

The behaviour of SSL\_connect() depends on the underlying BIO.

If the underlying BIO is **blocking**, SSL\_connect() will only return once the
handshake has been finished or an error occurred.

If the underlying BIO is **non-blocking**, SSL\_connect() will also return
when the underlying BIO could not satisfy the needs of SSL\_connect()
to continue the handshake, indicating the problem by the return value -1.
In this case a call to SSL\_get\_error() with the
return value of SSL\_connect() will yield **SSL\_ERROR\_WANT\_READ** or
**SSL\_ERROR\_WANT\_WRITE**. The calling process then must repeat the call after
taking appropriate action to satisfy the needs of SSL\_connect().
The action depends on the underlying BIO. When using a non-blocking socket,
nothing is to be done, but select() can be used to check for the required
condition. When using a buffering BIO, like a BIO pair, data must be written
into or retrieved out of the BIO before being able to continue.

# RETURN VALUES

The following return values can occur:

- 0

    The TLS/SSL handshake was not successful but was shut down controlled and
    by the specifications of the TLS/SSL protocol. Call SSL\_get\_error() with the
    return value **ret** to find out the reason.

- 1

    The TLS/SSL handshake was successfully completed, a TLS/SSL connection has been
    established.

- <0

    The TLS/SSL handshake was not successful, because a fatal error occurred either
    at the protocol level or a connection failure occurred. The shutdown was
    not clean. It can also occur of action is need to continue the operation
    for non-blocking BIOs. Call SSL\_get\_error() with the return value **ret**
    to find out the reason.

# SEE ALSO

[SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error), [SSL\_accept(3)](http://man.he.net/man3/SSL_accept),
[SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown), [ssl(3)](http://man.he.net/man3/ssl), [bio(3)](http://man.he.net/man3/bio),
[SSL\_set\_connect\_state(3)](http://man.he.net/man3/SSL_set_connect_state),
[SSL\_do\_handshake(3)](http://man.he.net/man3/SSL_do_handshake),
[SSL\_CTX\_new(3)](http://man.he.net/man3/SSL_CTX_new)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
