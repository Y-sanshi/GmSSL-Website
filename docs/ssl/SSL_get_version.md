# NAME

SSL\_get\_version, SSL\_is\_dtls - get the protocol information of a connection

# SYNOPSIS

    #include <openssl/ssl.h>

    const char *SSL_get_version(const SSL *ssl);

    int SSL_is_dtls(const SSL *ssl);

# DESCRIPTION

SSL\_get\_version() returns the name of the protocol used for the
connection **ssl**.

SSL\_is\_dtls() returns one if the connection is using DTLS, zero if not.

# RETURN VALUES

SSL\_get\_version() returns one of the following strings:

- SSLv3

    The connection uses the SSLv3 protocol.

- TLSv1

    The connection uses the TLSv1.0 protocol.

- TLSv1.1

    The connection uses the TLSv1.1 protocol.

- TLSv1.2

    The connection uses the TLSv1.2 protocol.

- unknown

    This indicates that no version has been set (no connection established).

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl)

# HISTORY

SSL\_is\_dtls() was added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
