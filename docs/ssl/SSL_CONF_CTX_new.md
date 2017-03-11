# NAME

SSL\_CONF\_CTX\_new, SSL\_CONF\_CTX\_free - SSL configuration allocation functions

# SYNOPSIS

    #include <openssl/ssl.h>

    SSL_CONF_CTX *SSL_CONF_CTX_new(void);
    void SSL_CONF_CTX_free(SSL_CONF_CTX *cctx);

# DESCRIPTION

The function SSL\_CONF\_CTX\_new() allocates and initialises an **SSL\_CONF\_CTX**
structure for use with the SSL\_CONF functions.

The function SSL\_CONF\_CTX\_free() frees up the context **cctx**.
If **cctx** is NULL nothing is done.

# RETURN VALUES

SSL\_CONF\_CTX\_new() returns either the newly allocated **SSL\_CONF\_CTX** structure
or **NULL** if an error occurs.

SSL\_CONF\_CTX\_free() does not return a value.

# SEE ALSO

[SSL\_CONF\_CTX\_set\_flags(3)](http://man.he.net/man3/SSL_CONF_CTX_set_flags),
[SSL\_CONF\_CTX\_set\_ssl\_ctx(3)](http://man.he.net/man3/SSL_CONF_CTX_set_ssl_ctx),
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
