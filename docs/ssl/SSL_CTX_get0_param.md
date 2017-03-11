# NAME

SSL\_CTX\_get0\_param, SSL\_get0\_param, SSL\_CTX\_set1\_param, SSL\_set1\_param -
get and set verification parameters

# SYNOPSIS

    #include <openssl/ssl.h>

    X509_VERIFY_PARAM *SSL_CTX_get0_param(SSL_CTX *ctx)
    X509_VERIFY_PARAM *SSL_get0_param(SSL *ssl)
    int SSL_CTX_set1_param(SSL_CTX *ctx, X509_VERIFY_PARAM *vpm)
    int SSL_set1_param(SSL *ssl, X509_VERIFY_PARAM *vpm)

# DESCRIPTION

SSL\_CTX\_get0\_param() and SSL\_get0\_param() retrieve an internal pointer to
the verification parameters for **ctx** or **ssl** respectively. The returned
pointer must not be freed by the calling application.

SSL\_CTX\_set1\_param() and SSL\_set1\_param() set the verification parameters
to **vpm** for **ctx** or **ssl**.

# NOTES

Typically parameters are retrieved from an **SSL\_CTX** or **SSL** structure
using SSL\_CTX\_get0\_param() or SSL\_get0\_param() and an application modifies
them to suit its needs: for example to add a hostname check.

# EXAMPLE

Check hostname matches "www.foo.com" in peer certificate:

    X509_VERIFY_PARAM *vpm = SSL_get0_param(ssl);
    X509_VERIFY_PARAM_set1_host(vpm, "www.foo.com", 0);

# RETURN VALUES

SSL\_CTX\_get0\_param() and SSL\_get0\_param() return a pointer to an
**X509\_VERIFY\_PARAM** structure.

SSL\_CTX\_set1\_param() and SSL\_set1\_param() return 1 for success and 0
for failure.

# SEE ALSO

[X509\_VERIFY\_PARAM\_set\_flags(3)](http://man.he.net/man3/X509_VERIFY_PARAM_set_flags)

# HISTORY

These functions were first added to OpenSSL 1.0.2.

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
