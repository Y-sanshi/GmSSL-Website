# NAME

SSL\_get\_peer\_certificate - get the X509 certificate of the peer

# SYNOPSIS

    #include <openssl/ssl.h>

    X509 *SSL_get_peer_certificate(const SSL *ssl);

# DESCRIPTION

SSL\_get\_peer\_certificate() returns a pointer to the X509 certificate the
peer presented. If the peer did not present a certificate, NULL is returned.

# NOTES

Due to the protocol definition, a TLS/SSL server will always send a
certificate, if present. A client will only send a certificate when
explicitly requested to do so by the server (see
[SSL\_CTX\_set\_verify(3)](http://man.he.net/man3/SSL_CTX_set_verify)). If an anonymous cipher
is used, no certificates are sent.

That a certificate is returned does not indicate information about the
verification state, use [SSL\_get\_verify\_result(3)](http://man.he.net/man3/SSL_get_verify_result)
to check the verification state.

The reference count of the X509 object is incremented by one, so that it
will not be destroyed when the session containing the peer certificate is
freed. The X509 object must be explicitly freed using X509\_free().

# RETURN VALUES

The following return values can occur:

- NULL

    No certificate was presented by the peer or no connection was established.

- Pointer to an X509 certificate

    The return value points to the certificate presented by the peer.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_get\_verify\_result(3)](http://man.he.net/man3/SSL_get_verify_result),
[SSL\_CTX\_set\_verify(3)](http://man.he.net/man3/SSL_CTX_set_verify)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
