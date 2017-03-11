# NAME

SSL\_CTX\_set\_cipher\_list, SSL\_set\_cipher\_list - choose list of available SSL\_CIPHERs

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CTX_set_cipher_list(SSL_CTX *ctx, const char *str);
    int SSL_set_cipher_list(SSL *ssl, const char *str);

# DESCRIPTION

SSL\_CTX\_set\_cipher\_list() sets the list of available ciphers for **ctx**
using the control string **str**. The format of the string is described
in [ciphers(1)](http://man.he.net/man1/ciphers). The list of ciphers is inherited by all
**ssl** objects created from **ctx**.

SSL\_set\_cipher\_list() sets the list of ciphers only for **ssl**.

# NOTES

The control string **str** should be universally usable and not depend
on details of the library configuration (ciphers compiled in). Thus no
syntax checking takes place. Items that are not recognized, because the
corresponding ciphers are not compiled in or because they are mistyped,
are simply ignored. Failure is only flagged if no ciphers could be collected
at all.

It should be noted, that inclusion of a cipher to be used into the list is
a necessary condition. On the client side, the inclusion into the list is
also sufficient unless the security level excludes it. On the server side,
additional restrictions apply. All ciphers have additional requirements.
ADH ciphers don't need a certificate, but DH-parameters must have been set.
All other ciphers need a corresponding certificate and key.

A RSA cipher can only be chosen, when a RSA certificate is available.
RSA ciphers using DHE need a certificate and key and additional DH-parameters
(see [SSL\_CTX\_set\_tmp\_dh\_callback(3)](http://man.he.net/man3/SSL_CTX_set_tmp_dh_callback)).

A DSA cipher can only be chosen, when a DSA certificate is available.
DSA ciphers always use DH key exchange and therefore need DH-parameters
(see [SSL\_CTX\_set\_tmp\_dh\_callback(3)](http://man.he.net/man3/SSL_CTX_set_tmp_dh_callback)).

When these conditions are not met for any cipher in the list (e.g. a
client only supports export RSA ciphers with an asymmetric key length
of 512 bits and the server is not configured to use temporary RSA
keys), the "no shared cipher" (SSL\_R\_NO\_SHARED\_CIPHER) error is generated
and the handshake will fail.

# RETURN VALUES

SSL\_CTX\_set\_cipher\_list() and SSL\_set\_cipher\_list() return 1 if any cipher
could be selected and 0 on complete failure.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_get\_ciphers(3)](http://man.he.net/man3/SSL_get_ciphers),
[SSL\_CTX\_use\_certificate(3)](http://man.he.net/man3/SSL_CTX_use_certificate),
[SSL\_CTX\_set\_tmp\_dh\_callback(3)](http://man.he.net/man3/SSL_CTX_set_tmp_dh_callback),
[ciphers(1)](http://man.he.net/man1/ciphers)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
