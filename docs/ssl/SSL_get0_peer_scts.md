# NAME

SSL\_get0\_peer\_scts - get SCTs received

# SYNOPSIS

    #include <openssl/ssl.h>

    const STACK_OF(SCT) *SSL_get0_peer_scts(SSL *s);

# DESCRIPTION

SSL\_get0\_peer\_scts() returns the signed certificate timestamps (SCTs) that have
been received. If this is the first time that this function has been called for
a given **SSL** instance, it will examine the TLS extensions, OCSP response and
the peer's certificate for SCTs. Future calls will return the same SCTs.

# RESTRICTIONS

If no Certificate Transparency validation callback has been set (using
**SSL\_CTX\_set\_ct\_validation\_callback** or **SSL\_set\_ct\_validation\_callback**),
this function is not guaranteed to return all of the SCTs that the peer is
capable of sending.

# RETURN VALUES

SSL\_get0\_peer\_scts() returns a list of SCTs found, or NULL if an error occurs.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_set\_ct\_validation\_callback(3)](http://man.he.net/man3/SSL_CTX_set_ct_validation_callback)

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
