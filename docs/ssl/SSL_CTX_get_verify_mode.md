# NAME

SSL\_CTX\_get\_verify\_mode, SSL\_get\_verify\_mode, SSL\_CTX\_get\_verify\_depth, SSL\_get\_verify\_depth, SSL\_get\_verify\_callback, SSL\_CTX\_get\_verify\_callback - get currently set verification parameters

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CTX_get_verify_mode(const SSL_CTX *ctx);
    int SSL_get_verify_mode(const SSL *ssl);
    int SSL_CTX_get_verify_depth(const SSL_CTX *ctx);
    int SSL_get_verify_depth(const SSL *ssl);
    int (*SSL_CTX_get_verify_callback(const SSL_CTX *ctx))(int, X509_STORE_CTX *);
    int (*SSL_get_verify_callback(const SSL *ssl))(int, X509_STORE_CTX *);

# DESCRIPTION

SSL\_CTX\_get\_verify\_mode() returns the verification mode currently set in
**ctx**.

SSL\_get\_verify\_mode() returns the verification mode currently set in
**ssl**.

SSL\_CTX\_get\_verify\_depth() returns the verification depth limit currently set
in **ctx**. If no limit has been explicitly set, -1 is returned and the
default value will be used.

SSL\_get\_verify\_depth() returns the verification depth limit currently set
in **ssl**. If no limit has been explicitly set, -1 is returned and the
default value will be used.

SSL\_CTX\_get\_verify\_callback() returns a function pointer to the verification
callback currently set in **ctx**. If no callback was explicitly set, the
NULL pointer is returned and the default callback will be used.

SSL\_get\_verify\_callback() returns a function pointer to the verification
callback currently set in **ssl**. If no callback was explicitly set, the
NULL pointer is returned and the default callback will be used.

# RETURN VALUES

See DESCRIPTION

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_CTX\_set\_verify(3)](http://man.he.net/man3/SSL_CTX_set_verify)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
