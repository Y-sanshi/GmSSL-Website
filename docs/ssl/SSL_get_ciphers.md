# NAME

SSL\_get1\_supported\_ciphers, SSL\_get\_client\_ciphers,
SSL\_get\_ciphers, SSL\_CTX\_get\_ciphers, SSL\_get\_cipher\_list
\- get list of available SSL\_CIPHERs

# SYNOPSIS

    #include <openssl/ssl.h>

    STACK_OF(SSL_CIPHER) *SSL_get_ciphers(const SSL *ssl);
    STACK_OF(SSL_CIPHER) *SSL_CTX_get_ciphers(const SSL_CTX *ctx);
    STACK_OF(SSL_CIPHER) *SSL_get1_supported_ciphers(SSL *s);
    STACK_OF(SSL_CIPHER) *SSL_get_client_ciphers(const SSL *ssl);
    const char *SSL_get_cipher_list(const SSL *ssl, int priority);

# DESCRIPTION

SSL\_get\_ciphers() returns the stack of available SSL\_CIPHERs for **ssl**,
sorted by preference. If **ssl** is NULL or no ciphers are available, NULL
is returned.

SSL\_CTX\_get\_ciphers() returns the stack of available SSL\_CIPHERs for **ctx**.

SSL\_get1\_supported\_ciphers() returns the stack of enabled SSL\_CIPHERs for
**ssl**, sorted by preference.
The list depends on settings like the cipher list, the supported protocol
versions, the security level, and the enabled signature algorithms.
SRP and PSK ciphers are only enabled if the appropriate callbacks or settings
have been applied.
This is the list that will be sent by the client to the server.
The list supported by the server might include more ciphers in case there is a
hole in the list of supported protocols.
The server will also not use ciphers from this list depending on the
configured certificates and DH parameters.
If **ssl** is NULL or no ciphers are available, NULL is returned.

SSL\_get\_client\_ciphers() returns the stack of available SSL\_CIPHERs matching the
list received from the client on **ssl**. If **ssl** is NULL, no ciphers are
available, or **ssl** is not operating in server mode, NULL is returned.

SSL\_get\_cipher\_list() returns a pointer to the name of the SSL\_CIPHER
listed for **ssl** with **priority**. If **ssl** is NULL, no ciphers are
available, or there are less ciphers than **priority** available, NULL
is returned.

# NOTES

The details of the ciphers obtained by SSL\_get\_ciphers(), SSL\_CTX\_get\_ciphers()
SSL\_get1\_supported\_ciphers() and SSL\_get\_client\_ciphers() can be obtained using
the [SSL\_CIPHER\_get\_name(3)](http://man.he.net/man3/SSL_CIPHER_get_name) family of functions.

Call SSL\_get\_cipher\_list() with **priority** starting from 0 to obtain the
sorted list of available ciphers, until NULL is returned.

Note: SSL\_get\_ciphers(), SSL\_CTX\_get\_ciphers() and SSL\_get\_client\_ciphers()
return a pointer to an internal cipher stack, which will be freed later on when
the SSL or SSL\_SESSION object is freed.  Therefore, the calling code **MUST NOT**
free the return value itself.

The stack returned by SSL\_get1\_supported\_ciphers() should be freed using
sk\_SSL\_CIPHER\_free().

# RETURN VALUES

See DESCRIPTION

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_CTX\_set\_cipher\_list(3)](http://man.he.net/man3/SSL_CTX_set_cipher_list),
[SSL\_CIPHER\_get\_name(3)](http://man.he.net/man3/SSL_CIPHER_get_name)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
