# NAME

SSL\_CTX\_set\_min\_proto\_version, SSL\_CTX\_set\_max\_proto\_version,
SSL\_set\_min\_proto\_version, SSL\_set\_max\_proto\_version - Set minimum
and maximum supported protocol version

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CTX_set_min_proto_version(SSL_CTX *ctx, int version);
    int SSL_CTX_set_max_proto_version(SSL_CTX *ctx, int version);
    int SSL_set_min_proto_version(SSL *ssl, int version);
    int SSL_set_max_proto_version(SSL *ssl, int version);

# DESCRIPTION

The functions set the minimum and maximum supported protocol versions
for the **ctx** or **ssl**.
This works in combination with the options set via
[SSL\_CTX\_set\_options(3)](http://man.he.net/man3/SSL_CTX_set_options) that also make it possible to disable
specific protocol versions.
Use these functions instead of disabling specific protocol versions.

Setting the minimum or maximum version to 0, will enable protocol
versions down to the lowest version, or up to the highest version
supported by the library, respectively.

Currently supported versions are **SSL3\_VERSION**, **TLS1\_VERSION**,
**TLS1\_1\_VERSION**, **TLS1\_2\_VERSION** for TLS and **DTLS1\_VERSION**,
**DTLS1\_2\_VERSION** for DTLS.

# RETURN VALUES

These functions return 1 on success and 0 on failure.

# NOTES

All these functions are implemented using macros.

# HISTORY

The functions were added in OpenSSL 1.1.0

# SEE ALSO

[SSL\_CTX\_set\_options(3)](http://man.he.net/man3/SSL_CTX_set_options), [SSL\_CONF\_cmd(3)](http://man.he.net/man3/SSL_CONF_cmd)

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
