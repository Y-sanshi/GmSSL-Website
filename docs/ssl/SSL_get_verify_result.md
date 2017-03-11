# NAME

SSL\_get\_verify\_result - get result of peer certificate verification

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_get_verify_result(const SSL *ssl);

# DESCRIPTION

SSL\_get\_verify\_result() returns the result of the verification of the
X509 certificate presented by the peer, if any.

# NOTES

SSL\_get\_verify\_result() can only return one error code while the verification
of a certificate can fail because of many reasons at the same time. Only
the last verification error that occurred during the processing is available
from SSL\_get\_verify\_result().

The verification result is part of the established session and is restored
when a session is reused.

# BUGS

If no peer certificate was presented, the returned result code is
X509\_V\_OK. This is because no verification error occurred, it does however
not indicate success. SSL\_get\_verify\_result() is only useful in connection
with [SSL\_get\_peer\_certificate(3)](http://man.he.net/man3/SSL_get_peer_certificate).

# RETURN VALUES

The following return values can currently occur:

- X509\_V\_OK

    The verification succeeded or no peer certificate was presented.

- Any other value

    Documented in [verify(1)](http://man.he.net/man1/verify).

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_set\_verify\_result(3)](http://man.he.net/man3/SSL_set_verify_result),
[SSL\_get\_peer\_certificate(3)](http://man.he.net/man3/SSL_get_peer_certificate),
[verify(1)](http://man.he.net/man1/verify)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
