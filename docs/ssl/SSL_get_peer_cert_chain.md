# NAME

SSL\_get\_peer\_cert\_chain, SSL\_get0\_verified\_chain - get the X509 certificate
chain of the peer

# SYNOPSIS

    #include <openssl/ssl.h>

    STACK_OF(X509) *SSL_get_peer_cert_chain(const SSL *ssl);
    STACK_OF(X509) *SSL_get0_verified_chain(const SSL *ssl);

# DESCRIPTION

SSL\_get\_peer\_cert\_chain() returns a pointer to STACK\_OF(X509) certificates
forming the certificate chain sent by the peer. If called on the client side,
the stack also contains the peer's certificate; if called on the server
side, the peer's certificate must be obtained separately using
[SSL\_get\_peer\_certificate(3)](http://man.he.net/man3/SSL_get_peer_certificate).
If the peer did not present a certificate, NULL is returned.

NB: SSL\_get\_peer\_cert\_chain() returns the peer chain as sent by the peer: it
only consists of certificates the peer has sent (in the order the peer
has sent them) it is **not** a verified chain.

SSL\_get0\_verified\_chain() returns the **verified** certificate chain
of the peer including the peer's end entity certificate. It must be called
after a session has been successfully established. If peer verification was
not successful (as indicated by SSL\_get\_verify\_result() not returning
X509\_V\_OK) the chain may be incomplete or invalid.

# NOTES

If the session is resumed peers do not send certificates so a NULL pointer
is returned by these functions. Applications can call SSL\_session\_reused()
to determine whether a session is resumed.

The reference count of each certificate in the returned STACK\_OF(X509) object
is not incremented and the returned stack may be invalidated by renegotiation.
If applications wish to use any certificates in the returned chain
indefinitely they must increase the reference counts using X509\_up\_ref() or
obtain a copy of the whole chain with X509\_chain\_up\_ref().

# RETURN VALUES

The following return values can occur:

- NULL

    No certificate was presented by the peer or no connection was established
    or the certificate chain is no longer available when a session is reused.

- Pointer to a STACK\_OF(X509)

    The return value points to the certificate chain presented by the peer.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_get\_peer\_certificate(3)](http://man.he.net/man3/SSL_get_peer_certificate), [X509\_up\_ref(3)](http://man.he.net/man3/X509_up_ref),
[X509\_chain\_up\_ref(3)](http://man.he.net/man3/X509_chain_up_ref)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
