# NAME

SSL\_library\_init, OpenSSL\_add\_ssl\_algorithms,
\- initialize SSL library by registering algorithms

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_library_init(void);

    int OpenSSL_add_ssl_algorithms(void);

# DESCRIPTION

SSL\_library\_init() registers the available SSL/TLS ciphers and digests.

OpenSSL\_add\_ssl\_algorithms() is a synonym for SSL\_library\_init() and is
implemented as a macro.

# NOTES

SSL\_library\_init() must be called before any other action takes place.
SSL\_library\_init() is not reentrant.

# WARNING

SSL\_library\_init() adds ciphers and digests used directly and indirectly by
SSL/TLS.

# RETURN VALUES

SSL\_library\_init() always returns "1", so it is safe to discard the return
value.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[RAND\_add(3)](http://man.he.net/man3/RAND_add)

# HISTORY

The SSL\_library\_init() and OpenSSL\_add\_ssl\_algorithms() functions were
deprecated in OpenSSL 1.1.0 by OPENSSL\_init\_ssl().

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
