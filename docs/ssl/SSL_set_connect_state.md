# NAME

SSL\_set\_connect\_state, SSL\_set\_accept\_state - prepare SSL object to work in client or server mode

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_set_connect_state(SSL *ssl);

    void SSL_set_accept_state(SSL *ssl);

# DESCRIPTION

SSL\_set\_connect\_state() sets **ssl** to work in client mode.

SSL\_set\_accept\_state() sets **ssl** to work in server mode.

# NOTES

When the SSL\_CTX object was created with [SSL\_CTX\_new(3)](http://man.he.net/man3/SSL_CTX_new),
it was either assigned a dedicated client method, a dedicated server
method, or a generic method, that can be used for both client and
server connections. (The method might have been changed with
[SSL\_CTX\_set\_ssl\_version(3)](http://man.he.net/man3/SSL_CTX_set_ssl_version) or
SSL\_set\_ssl\_method().)

When beginning a new handshake, the SSL engine must know whether it must
call the connect (client) or accept (server) routines. Even though it may
be clear from the method chosen, whether client or server mode was
requested, the handshake routines must be explicitly set.

When using the [SSL\_connect(3)](http://man.he.net/man3/SSL_connect) or
[SSL\_accept(3)](http://man.he.net/man3/SSL_accept) routines, the correct handshake
routines are automatically set. When performing a transparent negotiation
using [SSL\_write(3)](http://man.he.net/man3/SSL_write) or [SSL\_read(3)](http://man.he.net/man3/SSL_read), the
handshake routines must be explicitly set in advance using either
SSL\_set\_connect\_state() or SSL\_set\_accept\_state().

# RETURN VALUES

SSL\_set\_connect\_state() and SSL\_set\_accept\_state() do not return diagnostic
information.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_new(3)](http://man.he.net/man3/SSL_new), [SSL\_CTX\_new(3)](http://man.he.net/man3/SSL_CTX_new),
L[SSL\_connect(3)](http://man.he.net/man3/SSL_connect), [SSL\_accept(3)](http://man.he.net/man3/SSL_accept),
[SSL\_write(3)](http://man.he.net/man3/SSL_write), [SSL\_read(3)](http://man.he.net/man3/SSL_read),
[SSL\_do\_handshake(3)](http://man.he.net/man3/SSL_do_handshake),
[SSL\_CTX\_set\_ssl\_version(3)](http://man.he.net/man3/SSL_CTX_set_ssl_version)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
