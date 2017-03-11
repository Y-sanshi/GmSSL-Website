# NAME

SSL\_CONF\_CTX\_set\_flags, SSL\_CONF\_CTX\_clear\_flags - Set of clear SSL configuration context flags

# SYNOPSIS

    #include <openssl/ssl.h>

    unsigned int SSL_CONF_CTX_set_flags(SSL_CONF_CTX *cctx, unsigned int flags);
    unsigned int SSL_CONF_CTX_clear_flags(SSL_CONF_CTX *cctx, unsigned int flags);

# DESCRIPTION

The function SSL\_CONF\_CTX\_set\_flags() sets **flags** in the context **cctx**.

The function SSL\_CONF\_CTX\_clear\_flags() clears **flags** in the context **cctx**.

# NOTES

The flags set affect how subsequent calls to SSL\_CONF\_cmd() or
SSL\_CONF\_argv() behave.

Currently the following **flags** values are recognised:

- SSL\_CONF\_FLAG\_CMDLINE, SSL\_CONF\_FLAG\_FILE

    recognise options intended for command line or configuration file use. At
    least one of these flags must be set.

- SSL\_CONF\_FLAG\_CLIENT, SSL\_CONF\_FLAG\_SERVER

    recognise options intended for use in SSL/TLS clients or servers. One or
    both of these flags must be set.

- SSL\_CONF\_FLAG\_CERTIFICATE

    recognise certificate and private key options.

- SSL\_CONF\_FLAG\_REQUIRE\_PRIVATE

    If this option is set then if a private key is not specified for a certificate
    it will attempt to load a private key from the certificate file when
    SSL\_CONF\_CTX\_finish() is called. If a key cannot be loaded from the certificate
    file an error occurs.

- SSL\_CONF\_FLAG\_SHOW\_ERRORS

    indicate errors relating to unrecognised options or missing arguments in
    the error queue. If this option isn't set such errors are only reflected
    in the return values of SSL\_CONF\_set\_cmd() or SSL\_CONF\_set\_argv()

# RETURN VALUES

SSL\_CONF\_CTX\_set\_flags() and SSL\_CONF\_CTX\_clear\_flags() returns the new flags
value after setting or clearing flags.

# SEE ALSO

[SSL\_CONF\_CTX\_new(3)](http://man.he.net/man3/SSL_CONF_CTX_new),
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
