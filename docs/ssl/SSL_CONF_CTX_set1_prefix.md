# NAME

SSL\_CONF\_CTX\_set1\_prefix - Set configuration context command prefix

# SYNOPSIS

    #include <openssl/ssl.h>

    unsigned int SSL_CONF_CTX_set1_prefix(SSL_CONF_CTX *cctx, const char *prefix);

# DESCRIPTION

The function SSL\_CONF\_CTX\_set1\_prefix() sets the command prefix of **cctx**
to **prefix**. If **prefix** is **NULL** it is restored to the default value.

# NOTES

Command prefixes alter the commands recognised by subsequent SSL\_CTX\_cmd()
calls. For example for files, if the prefix "SSL" is set then command names
such as "SSLProtocol", "SSLOptions" etc. are recognised instead of "Protocol"
and "Options". Similarly for command lines if the prefix is "--ssl-" then
"--ssl-no\_tls1\_2" is recognised instead of "-no\_tls1\_2".

If the **SSL\_CONF\_FLAG\_CMDLINE** flag is set then prefix checks are case
sensitive and "-" is the default. In the unlikely even an application
explicitly wants to set no prefix it must be explicitly set to "".

If the **SSL\_CONF\_FLAG\_FILE** flag is set then prefix checks are case
insensitive and no prefix is the default.

# RETURN VALUES

SSL\_CONF\_CTX\_set1\_prefix() returns 1 for success and 0 for failure.

# SEE ALSO

[SSL\_CONF\_CTX\_new(3)](http://man.he.net/man3/SSL_CONF_CTX_new),
[SSL\_CONF\_CTX\_set\_flags(3)](http://man.he.net/man3/SSL_CONF_CTX_set_flags),
[SSL\_CONF\_CTX\_set\_ssl\_ctx(3)](http://man.he.net/man3/SSL_CONF_CTX_set_ssl_ctx),
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
