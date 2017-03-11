# NAME

SSL\_CTX\_set\_quiet\_shutdown, SSL\_CTX\_get\_quiet\_shutdown, SSL\_set\_quiet\_shutdown, SSL\_get\_quiet\_shutdown - manipulate shutdown behaviour

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_CTX_set_quiet_shutdown(SSL_CTX *ctx, int mode);
    int SSL_CTX_get_quiet_shutdown(const SSL_CTX *ctx);

    void SSL_set_quiet_shutdown(SSL *ssl, int mode);
    int SSL_get_quiet_shutdown(const SSL *ssl);

# DESCRIPTION

SSL\_CTX\_set\_quiet\_shutdown() sets the "quiet shutdown" flag for **ctx** to be
**mode**. SSL objects created from **ctx** inherit the **mode** valid at the time
[SSL\_new(3)](http://man.he.net/man3/SSL_new) is called. **mode** may be 0 or 1.

SSL\_CTX\_get\_quiet\_shutdown() returns the "quiet shutdown" setting of **ctx**.

SSL\_set\_quiet\_shutdown() sets the "quiet shutdown" flag for **ssl** to be
**mode**. The setting stays valid until **ssl** is removed with
[SSL\_free(3)](http://man.he.net/man3/SSL_free) or SSL\_set\_quiet\_shutdown() is called again.
It is not changed when [SSL\_clear(3)](http://man.he.net/man3/SSL_clear) is called.
**mode** may be 0 or 1.

SSL\_get\_quiet\_shutdown() returns the "quiet shutdown" setting of **ssl**.

# NOTES

Normally when a SSL connection is finished, the parties must send out
"close notify" alert messages using [SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown)
for a clean shutdown.

When setting the "quiet shutdown" flag to 1, [SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown)
will set the internal flags to SSL\_SENT\_SHUTDOWN|SSL\_RECEIVED\_SHUTDOWN.
([SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown) then behaves like
[SSL\_set\_shutdown(3)](http://man.he.net/man3/SSL_set_shutdown) called with
SSL\_SENT\_SHUTDOWN|SSL\_RECEIVED\_SHUTDOWN.)
The session is thus considered to be shutdown, but no "close notify" alert
is sent to the peer. This behaviour violates the TLS standard.

The default is normal shutdown behaviour as described by the TLS standard.

# RETURN VALUES

SSL\_CTX\_set\_quiet\_shutdown() and SSL\_set\_quiet\_shutdown() do not return
diagnostic information.

SSL\_CTX\_get\_quiet\_shutdown() and SSL\_get\_quiet\_shutdown return the current
setting.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown),
[SSL\_set\_shutdown(3)](http://man.he.net/man3/SSL_set_shutdown), [SSL\_new(3)](http://man.he.net/man3/SSL_new),
[SSL\_clear(3)](http://man.he.net/man3/SSL_clear), [SSL\_free(3)](http://man.he.net/man3/SSL_free)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
