# NAME

SSL\_CTX\_ctrl, SSL\_CTX\_callback\_ctrl, SSL\_ctrl, SSL\_callback\_ctrl - internal handling functions for SSL\_CTX and SSL objects

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_CTX_ctrl(SSL_CTX *ctx, int cmd, long larg, void *parg);
    long SSL_CTX_callback_ctrl(SSL_CTX *, int cmd, void (*fp)());

    long SSL_ctrl(SSL *ssl, int cmd, long larg, void *parg);
    long SSL_callback_ctrl(SSL *, int cmd, void (*fp)());

# DESCRIPTION

The SSL\_\*\_ctrl() family of functions is used to manipulate settings of
the SSL\_CTX and SSL objects. Depending on the command **cmd** the arguments
**larg**, **parg**, or **fp** are evaluated. These functions should never
be called directly. All functionalities needed are made available via
other functions or macros.

# RETURN VALUES

The return values of the SSL\*\_ctrl() functions depend on the command
supplied via the **cmd** parameter.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
