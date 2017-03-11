# NAME

SSL\_CTX\_set\_read\_ahead, SSL\_CTX\_get\_read\_ahead,
SSL\_set\_read\_ahead, SSL\_get\_read\_ahead,
SSL\_CTX\_get\_default\_read\_ahead
\- manage whether to read as many input bytes as possible

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_set_read_ahead(SSL *s, int yes);
    int SSL_get_read_ahead(const SSL *s);

    SSL_CTX_set_read_ahead(SSL_CTX *ctx, int yes);
    long SSL_CTX_get_read_ahead(SSL_CTX *ctx);
    long SSL_CTX_get_default_read_ahead(SSL_CTX *ctx);

# DESCRIPTION

SSL\_CTX\_set\_read\_ahead() and SSL\_set\_read\_ahead() set whether we should read as
many input bytes as possible (for non-blocking reads) or not. For example if
**x** bytes are currently required by OpenSSL, but **y** bytes are available from
the underlying BIO (where **y** > **x**), then OpenSSL will read all **y** bytes
into its buffer (providing that the buffer is large enough) if reading ahead is
on, or **x** bytes otherwise. The parameter **yes** or **m** should be 0 to ensure
reading ahead is off, or non zero otherwise.
SSL\_CTX\_set\_default\_read\_ahead() is identical to SSL\_CTX\_set\_read\_ahead().

SSL\_CTX\_get\_read\_ahead() and SSL\_get\_read\_ahead() indicate whether reading
ahead has been set or not.

# NOTES

These functions have no impact when used with DTLS. The return values for
SSL\_CTX\_get\_read\_head() and SSL\_get\_read\_ahead() are undefined for DTLS. Setting
**read\_ahead** can impact the behaviour of the SSL\_pending() function
(see [SSL\_pending(3)](http://man.he.net/man3/SSL_pending)).

# RETURN VALUES

SSL\_get\_read\_ahead() and SSL\_CTX\_get\_read\_ahead() return 0 if reading ahead is off,
and non zero otherwise.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_pending(3)](http://man.he.net/man3/SSL_pending)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
