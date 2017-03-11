# NAME

SSL\_SESSION\_get\_protocol\_version - retrieve session protocol version

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_SESSION_get_protocol_version(const SSL_SESSION *s);

# DESCRIPTION

SSL\_SESSION\_get\_protocol\_version() returns the protocol version number used
by session **s**.

# RETURN VALUES

SSL\_SESSION\_get\_protocol\_version() returns a number indicating the protocol
version used for the session; this number matches the constants _e.g._
**TLS1\_VERSION** or **TLS1\_2\_VERSION**.

Note that the SSL\_SESSION\_get\_protocol\_version() function
does **not** perform a null check on the provided session **s** pointer.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl)

# HISTORY

SSL\_SESSION\_get\_protocol\_version() was first added to OpenSSL 1.1.0

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
