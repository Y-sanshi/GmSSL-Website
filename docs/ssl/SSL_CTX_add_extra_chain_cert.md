# NAME

SSL\_CTX\_add\_extra\_chain\_cert, SSL\_CTX\_clear\_extra\_chain\_certs - add or clear
extra chain certificates

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_CTX_add_extra_chain_cert(SSL_CTX *ctx, X509 *x509);
    long SSL_CTX_clear_extra_chain_certs(SSL_CTX *ctx);

# DESCRIPTION

SSL\_CTX\_add\_extra\_chain\_cert() adds the certificate **x509** to the extra chain
certificates associated with **ctx**. Several certificates can be added one
after another.

SSL\_CTX\_clear\_extra\_chain\_certs() clears all extra chain certificates
associated with **ctx**.

These functions are implemented as macros.

# NOTES

When sending a certificate chain, extra chain certificates are sent in order
following the end entity certificate.

If no chain is specified, the library will try to complete the chain from the
available CA certificates in the trusted CA storage, see
[SSL\_CTX\_load\_verify\_locations(3)](http://man.he.net/man3/SSL_CTX_load_verify_locations).

The **x509** certificate provided to SSL\_CTX\_add\_extra\_chain\_cert() will be
freed by the library when the **SSL\_CTX** is destroyed. An application
**should not** free the **x509** object.

# RESTRICTIONS

Only one set of extra chain certificates can be specified per SSL\_CTX
structure. Different chains for different certificates (for example if both
RSA and DSA certificates are specified by the same server) or different SSL
structures with the same parent SSL\_CTX cannot be specified using this
function. For more flexibility functions such as SSL\_add1\_chain\_cert() should
be used instead.

# RETURN VALUES

SSL\_CTX\_add\_extra\_chain\_cert() and SSL\_CTX\_clear\_extra\_chain\_certs() return
1 on success and 0 for failure. Check out the error stack to find out the
reason for failure.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_use\_certificate(3)](http://man.he.net/man3/SSL_CTX_use_certificate),
[SSL\_CTX\_set\_client\_cert\_cb(3)](http://man.he.net/man3/SSL_CTX_set_client_cert_cb),
[SSL\_CTX\_load\_verify\_locations(3)](http://man.he.net/man3/SSL_CTX_load_verify_locations)
[SSL\_CTX\_set0\_chain(3)](http://man.he.net/man3/SSL_CTX_set0_chain)
[SSL\_CTX\_set1\_chain(3)](http://man.he.net/man3/SSL_CTX_set1_chain)
[SSL\_CTX\_add0\_chain\_cert(3)](http://man.he.net/man3/SSL_CTX_add0_chain_cert)
[SSL\_CTX\_add1\_chain\_cert(3)](http://man.he.net/man3/SSL_CTX_add1_chain_cert)
[SSL\_set0\_chain(3)](http://man.he.net/man3/SSL_set0_chain)
[SSL\_set1\_chain(3)](http://man.he.net/man3/SSL_set1_chain)
[SSL\_add0\_chain\_cert(3)](http://man.he.net/man3/SSL_add0_chain_cert)
[SSL\_add1\_chain\_cert(3)](http://man.he.net/man3/SSL_add1_chain_cert)
[SSL\_CTX\_build\_cert\_chain(3)](http://man.he.net/man3/SSL_CTX_build_cert_chain)
[SSL\_build\_cert\_chain(3)](http://man.he.net/man3/SSL_build_cert_chain)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
