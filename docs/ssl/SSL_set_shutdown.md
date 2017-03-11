# NAME

SSL\_set\_shutdown, SSL\_get\_shutdown - manipulate shutdown state of an SSL connection

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_set_shutdown(SSL *ssl, int mode);

    int SSL_get_shutdown(const SSL *ssl);

# DESCRIPTION

SSL\_set\_shutdown() sets the shutdown state of **ssl** to **mode**.

SSL\_get\_shutdown() returns the shutdown mode of **ssl**.

# NOTES

The shutdown state of an ssl connection is a bitmask of:

- 0

    No shutdown setting, yet.

- SSL\_SENT\_SHUTDOWN

    A "close notify" shutdown alert was sent to the peer, the connection is being
    considered closed and the session is closed and correct.

- SSL\_RECEIVED\_SHUTDOWN

    A shutdown alert was received form the peer, either a normal "close notify"
    or a fatal error.

SSL\_SENT\_SHUTDOWN and SSL\_RECEIVED\_SHUTDOWN can be set at the same time.

The shutdown state of the connection is used to determine the state of
the ssl session. If the session is still open, when
[SSL\_clear(3)](http://man.he.net/man3/SSL_clear) or [SSL\_free(3)](http://man.he.net/man3/SSL_free) is called,
it is considered bad and removed according to RFC2246.
The actual condition for a correctly closed session is SSL\_SENT\_SHUTDOWN
(according to the TLS RFC, it is acceptable to only send the "close notify"
alert but to not wait for the peer's answer, when the underlying connection
is closed).
SSL\_set\_shutdown() can be used to set this state without sending a
close alert to the peer (see [SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown)).

If a "close notify" was received, SSL\_RECEIVED\_SHUTDOWN will be set,
for setting SSL\_SENT\_SHUTDOWN the application must however still call
[SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown) or SSL\_set\_shutdown() itself.

# RETURN VALUES

SSL\_set\_shutdown() does not return diagnostic information.

SSL\_get\_shutdown() returns the current setting.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown),
[SSL\_CTX\_set\_quiet\_shutdown(3)](http://man.he.net/man3/SSL_CTX_set_quiet_shutdown),
[SSL\_clear(3)](http://man.he.net/man3/SSL_clear), [SSL\_free(3)](http://man.he.net/man3/SSL_free)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
