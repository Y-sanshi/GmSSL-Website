# NAME

SSL\_shutdown - shut down a TLS/SSL connection

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_shutdown(SSL *ssl);

# DESCRIPTION

SSL\_shutdown() shuts down an active TLS/SSL connection. It sends the
"close notify" shutdown alert to the peer.

# NOTES

SSL\_shutdown() tries to send the "close notify" shutdown alert to the peer.
Whether the operation succeeds or not, the SSL\_SENT\_SHUTDOWN flag is set and
a currently open session is considered closed and good and will be kept in the
session cache for further reuse.

The shutdown procedure consists of 2 steps: the sending of the "close notify"
shutdown alert and the reception of the peer's "close notify" shutdown
alert. According to the TLS standard, it is acceptable for an application
to only send its shutdown alert and then close the underlying connection
without waiting for the peer's response (this way resources can be saved,
as the process can already terminate or serve another connection).
When the underlying connection shall be used for more communications, the
complete shutdown procedure (bidirectional "close notify" alerts) must be
performed, so that the peers stay synchronized.

SSL\_shutdown() supports both uni- and bidirectional shutdown by its 2 step
behaviour.

- When the application is the first party to send the "close notify"
alert, SSL\_shutdown() will only send the alert and then set the
SSL\_SENT\_SHUTDOWN flag (so that the session is considered good and will
be kept in cache). SSL\_shutdown() will then return with 0. If a unidirectional
shutdown is enough (the underlying connection shall be closed anyway), this
first call to SSL\_shutdown() is sufficient. In order to complete the
bidirectional shutdown handshake, SSL\_shutdown() must be called again.
The second call will make SSL\_shutdown() wait for the peer's "close notify"
shutdown alert. On success, the second call to SSL\_shutdown() will return
with 1.
- If the peer already sent the "close notify" alert **and** it was
already processed implicitly inside another function
([SSL\_read(3)](http://man.he.net/man3/SSL_read)), the SSL\_RECEIVED\_SHUTDOWN flag is set.
SSL\_shutdown() will send the "close notify" alert, set the SSL\_SENT\_SHUTDOWN
flag and will immediately return with 1.
Whether SSL\_RECEIVED\_SHUTDOWN is already set can be checked using the
SSL\_get\_shutdown() (see also [SSL\_set\_shutdown(3)](http://man.he.net/man3/SSL_set_shutdown) call.

It is therefore recommended, to check the return value of SSL\_shutdown()
and call SSL\_shutdown() again, if the bidirectional shutdown is not yet
complete (return value of the first call is 0).

The behaviour of SSL\_shutdown() additionally depends on the underlying BIO.

If the underlying BIO is **blocking**, SSL\_shutdown() will only return once the
handshake step has been finished or an error occurred.

If the underlying BIO is **non-blocking**, SSL\_shutdown() will also return
when the underlying BIO could not satisfy the needs of SSL\_shutdown()
to continue the handshake. In this case a call to SSL\_get\_error() with the
return value of SSL\_shutdown() will yield **SSL\_ERROR\_WANT\_READ** or
**SSL\_ERROR\_WANT\_WRITE**. The calling process then must repeat the call after
taking appropriate action to satisfy the needs of SSL\_shutdown().
The action depends on the underlying BIO. When using a non-blocking socket,
nothing is to be done, but select() can be used to check for the required
condition. When using a buffering BIO, like a BIO pair, data must be written
into or retrieved out of the BIO before being able to continue.

SSL\_shutdown() can be modified to only set the connection to "shutdown"
state but not actually send the "close notify" alert messages,
see [SSL\_CTX\_set\_quiet\_shutdown(3)](http://man.he.net/man3/SSL_CTX_set_quiet_shutdown).
When "quiet shutdown" is enabled, SSL\_shutdown() will always succeed
and return 1.

# RETURN VALUES

The following return values can occur:

- 0

    The shutdown is not yet finished. Call SSL\_shutdown() for a second time,
    if a bidirectional shutdown shall be performed.
    The output of [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error) may be misleading, as an
    erroneous SSL\_ERROR\_SYSCALL may be flagged even though no error occurred.

- 1

    The shutdown was successfully completed. The "close notify" alert was sent
    and the peer's "close notify" alert was received.

- <0

    The shutdown was not successful because a fatal error occurred either
    at the protocol level or a connection failure occurred. It can also occur if
    action is need to continue the operation for non-blocking BIOs.
    Call [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error) with the return value **ret**
    to find out the reason.

# SEE ALSO

[SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error), [SSL\_connect(3)](http://man.he.net/man3/SSL_connect),
[SSL\_accept(3)](http://man.he.net/man3/SSL_accept), [SSL\_set\_shutdown(3)](http://man.he.net/man3/SSL_set_shutdown),
[SSL\_CTX\_set\_quiet\_shutdown(3)](http://man.he.net/man3/SSL_CTX_set_quiet_shutdown),
[SSL\_clear(3)](http://man.he.net/man3/SSL_clear), [SSL\_free(3)](http://man.he.net/man3/SSL_free),
[ssl(3)](http://man.he.net/man3/ssl), [bio(3)](http://man.he.net/man3/bio)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
