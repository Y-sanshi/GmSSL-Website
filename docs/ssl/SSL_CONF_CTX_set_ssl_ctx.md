# NAME

SSL\_CONF\_CTX\_set\_ssl\_ctx, SSL\_CONF\_CTX\_set\_ssl - set context to configure

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_CONF_CTX_set_ssl_ctx(SSL_CONF_CTX *cctx, SSL_CTX *ctx);
    void SSL_CONF_CTX_set_ssl(SSL_CONF_CTX *cctx, SSL *ssl);

# DESCRIPTION

SSL\_CONF\_CTX\_set\_ssl\_ctx() sets the context associated with **cctx** to the
**SSL\_CTX** structure **ctx**. Any previous **SSL** or **SSL\_CTX** associated with
**cctx** is cleared. Subsequent calls to SSL\_CONF\_cmd() will be sent to
**ctx**.

SSL\_CONF\_CTX\_set\_ssl() sets the context associated with **cctx** to the
**SSL** structure **ssl**. Any previous **SSL** or **SSL\_CTX** associated with
**cctx** is cleared. Subsequent calls to SSL\_CONF\_cmd() will be sent to
**ssl**.

# NOTES

The context need not be set or it can be set to **NULL** in which case only
syntax checking of commands is performed, where possible.

# RETURN VALUES

SSL\_CONF\_CTX\_set\_ssl\_ctx() and SSL\_CTX\_set\_ssl() do not return a value.

# SEE ALSO

[SSL\_CONF\_CTX\_new(3)](http://man.he.net/man3/SSL_CONF_CTX_new),
[SSL\_CONF\_CTX\_set\_flags(3)](http://man.he.net/man3/SSL_CONF_CTX_set_flags),
[SSL\_CONF\_CTX\_set1\_prefix(3)](http://man.he.net/man3/SSL_CONF_CTX_set1_prefix),
[SSL\_CONF\_cmd(3)](http://man.he.net/man3/SSL_CONF_cmd),
[SSL\_CONF\_cmd\_argv(3)](http://man.he.net/man3/SSL_CONF_cmd_argv)

# HISTORY

These functions were first added to OpenSSL 1.0.2

# COPYRIGHT

Copyright 2012-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
