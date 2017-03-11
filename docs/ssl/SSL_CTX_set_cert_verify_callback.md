# NAME

SSL\_CTX\_set\_cert\_verify\_callback - set peer certificate verification procedure

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_CTX_set_cert_verify_callback(SSL_CTX *ctx, int (*callback)(X509_STORE_CTX *, void *), void *arg);

# DESCRIPTION

SSL\_CTX\_set\_cert\_verify\_callback() sets the verification callback function for
_ctx_. SSL objects that are created from _ctx_ inherit the setting valid at
the time when [SSL\_new(3)](http://man.he.net/man3/SSL_new) is called.

# NOTES

Whenever a certificate is verified during a SSL/TLS handshake, a verification
function is called. If the application does not explicitly specify a
verification callback function, the built-in verification function is used.
If a verification callback _callback_ is specified via
SSL\_CTX\_set\_cert\_verify\_callback(), the supplied callback function is called
instead. By setting _callback_ to NULL, the default behaviour is restored.

When the verification must be performed, _callback_ will be called with
the arguments callback(X509\_STORE\_CTX \*x509\_store\_ctx, void \*arg). The
argument _arg_ is specified by the application when setting _callback_.

_callback_ should return 1 to indicate verification success and 0 to
indicate verification failure. If SSL\_VERIFY\_PEER is set and _callback_
returns 0, the handshake will fail. As the verification procedure may
allow to continue the connection in case of failure (by always returning 1)
the verification result must be set in any case using the **error**
member of _x509\_store\_ctx_ so that the calling application will be informed
about the detailed result of the verification procedure!

Within _x509\_store\_ctx_, _callback_ has access to the _verify\_callback_
function set using [SSL\_CTX\_set\_verify(3)](http://man.he.net/man3/SSL_CTX_set_verify).

# WARNINGS

Do not mix the verification callback described in this function with the
**verify\_callback** function called during the verification process. The
latter is set using the [SSL\_CTX\_set\_verify(3)](http://man.he.net/man3/SSL_CTX_set_verify)
family of functions.

Providing a complete verification procedure including certificate purpose
settings etc is a complex task. The built-in procedure is quite powerful
and in most cases it should be sufficient to modify its behaviour using
the **verify\_callback** function.

# BUGS

SSL\_CTX\_set\_cert\_verify\_callback() does not provide diagnostic information.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_CTX\_set\_verify(3)](http://man.he.net/man3/SSL_CTX_set_verify),
[SSL\_get\_verify\_result(3)](http://man.he.net/man3/SSL_get_verify_result),
[SSL\_CTX\_load\_verify\_locations(3)](http://man.he.net/man3/SSL_CTX_load_verify_locations)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
