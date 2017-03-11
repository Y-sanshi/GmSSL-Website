# NAME

SSL\_CONF\_cmd\_argv - SSL configuration command line processing

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CONF_cmd_argv(SSL_CONF_CTX *cctx, int *pargc, char ***pargv);

# DESCRIPTION

The function SSL\_CONF\_cmd\_argv() processes at most two command line
arguments from **pargv** and **pargc**. The values of **pargv** and **pargc**
are updated to reflect the number of command options processed. The **pargc**
argument can be set to **NULL** is it is not used.

# RETURN VALUES

SSL\_CONF\_cmd\_argv() returns the number of command arguments processed: 0, 1, 2
or a negative error code.

If -2 is returned then an argument for a command is missing.

If -1 is returned the command is recognised but couldn't be processed due
to an error: for example a syntax error in the argument.

# SEE ALSO

[SSL\_CONF\_CTX\_new(3)](http://man.he.net/man3/SSL_CONF_CTX_new),
[SSL\_CONF\_CTX\_set\_flags(3)](http://man.he.net/man3/SSL_CONF_CTX_set_flags),
[SSL\_CONF\_CTX\_set1\_prefix(3)](http://man.he.net/man3/SSL_CONF_CTX_set1_prefix),
[SSL\_CONF\_CTX\_set\_ssl\_ctx(3)](http://man.he.net/man3/SSL_CONF_CTX_set_ssl_ctx),
[SSL\_CONF\_cmd(3)](http://man.he.net/man3/SSL_CONF_cmd)

# HISTORY

These functions were first added to OpenSSL 1.0.2

# COPYRIGHT

Copyright 2012-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
