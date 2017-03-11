# NAME

SSL\_CTX\_set\_msg\_callback, SSL\_CTX\_set\_msg\_callback\_arg, SSL\_set\_msg\_callback, SSL\_set\_msg\_callback\_arg - install callback for observing protocol messages

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_CTX_set_msg_callback(SSL_CTX *ctx, void (*cb)(int write_p, int version, int content_type, const void *buf, size_t len, SSL *ssl, void *arg));
    void SSL_CTX_set_msg_callback_arg(SSL_CTX *ctx, void *arg);

    void SSL_set_msg_callback(SSL *ssl, void (*cb)(int write_p, int version, int content_type, const void *buf, size_t len, SSL *ssl, void *arg));
    void SSL_set_msg_callback_arg(SSL *ssl, void *arg);

# DESCRIPTION

SSL\_CTX\_set\_msg\_callback() or SSL\_set\_msg\_callback() can be used to
define a message callback function _cb_ for observing all SSL/TLS
protocol messages (such as handshake messages) that are received or
sent.  SSL\_CTX\_set\_msg\_callback\_arg() and SSL\_set\_msg\_callback\_arg()
can be used to set argument _arg_ to the callback function, which is
available for arbitrary application use.

SSL\_CTX\_set\_msg\_callback() and SSL\_CTX\_set\_msg\_callback\_arg() specify
default settings that will be copied to new **SSL** objects by
[SSL\_new(3)](http://man.he.net/man3/SSL_new). SSL\_set\_msg\_callback() and
SSL\_set\_msg\_callback\_arg() modify the actual settings of an **SSL**
object. Using a **0** pointer for _cb_ disables the message callback.

When _cb_ is called by the SSL/TLS library for a protocol message,
the function arguments have the following meaning:

- _write\_p_

    This flag is **0** when a protocol message has been received and **1**
    when a protocol message has been sent.

- _version_

    The protocol version according to which the protocol message is
    interpreted by the library. Currently, this is one of
    **SSL2\_VERSION**, **SSL3\_VERSION** and **TLS1\_VERSION** (for SSL 2.0, SSL
    3.0 and TLS 1.0, respectively).

- _content\_type_

    In the case of SSL 2.0, this is always **0**.  In the case of SSL 3.0
    or TLS 1.0, this is one of the **ContentType** values defined in the
    protocol specification (**change\_cipher\_spec(20)**, **alert(21)**,
    **handshake(22)**; but never **application\_data(23)** because the
    callback will only be called for protocol messages).

- _buf_, _len_

    _buf_ points to a buffer containing the protocol message, which
    consists of _len_ bytes. The buffer is no longer valid after the
    callback function has returned.

- _ssl_

    The **SSL** object that received or sent the message.

- _arg_

    The user-defined argument optionally defined by
    SSL\_CTX\_set\_msg\_callback\_arg() or SSL\_set\_msg\_callback\_arg().

# NOTES

Protocol messages are passed to the callback function after decryption
and fragment collection where applicable. (Thus record boundaries are
not visible.)

If processing a received protocol message results in an error,
the callback function may not be called.  For example, the callback
function will never see messages that are considered too large to be
processed.

Due to automatic protocol version negotiation, _version_ is not
necessarily the protocol version used by the sender of the message: If
a TLS 1.0 ClientHello message is received by an SSL 3.0-only server,
_version_ will be **SSL3\_VERSION**.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_new(3)](http://man.he.net/man3/SSL_new)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
