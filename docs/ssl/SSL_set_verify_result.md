# NAME

SSL\_set\_verify\_result - override result of peer certificate verification

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_set_verify_result(SSL *ssl, long verify_result);

# DESCRIPTION

SSL\_set\_verify\_result() sets **verify\_result** of the object **ssl** to be the
result of the verification of the X509 certificate presented by the peer,
if any.

# NOTES

SSL\_set\_verify\_result() overrides the verification result. It only changes
the verification result of the **ssl** object. It does not become part of the
established session, so if the session is to be reused later, the original
value will reappear.

The valid codes for **verify\_result** are documented in [verify(1)](http://man.he.net/man1/verify).

# RETURN VALUES

SSL\_set\_verify\_result() does not provide a return value.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_get\_verify\_result(3)](http://man.he.net/man3/SSL_get_verify_result),
[SSL\_get\_peer\_certificate(3)](http://man.he.net/man3/SSL_get_peer_certificate),
[verify(1)](http://man.he.net/man1/verify)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
