# NAME

ERR\_load\_crypto\_strings, SSL\_load\_error\_strings, ERR\_free\_strings -
load and free error strings

# SYNOPSIS

Deprecated:

    #include <openssl/err.h>

    #if OPENSSL_API_COMPAT < 0x10100000L
    void ERR_load_crypto_strings(void);
    void ERR_free_strings(void);
    #endif

    #include <openssl/ssl.h>

    #if OPENSSL_API_COMPAT < 0x10100000L
    void SSL_load_error_strings(void);
    #endif

# DESCRIPTION

All of the following functions are deprecated from OpenSSL 1.1.0. No explicit
initialisation or de-initialisation is necessary. See [OPENSSL\_init\_crypto(3)](http://man.he.net/man3/OPENSSL_init_crypto)
and [OPENSSL\_init\_ssl(3)](http://man.he.net/man3/OPENSSL_init_ssl).

ERR\_load\_crypto\_strings() registers the error strings for all
**libcrypto** functions. SSL\_load\_error\_strings() does the same,
but also registers the **libssl** error strings.

In versions of OpenSSL prior to 1.1.0 ERR\_free\_strings() freed all previously
loaded error strings. However from OpenSSL 1.1.0 it does nothing.

# RETURN VALUES

ERR\_load\_crypto\_strings(), SSL\_load\_error\_strings() and
ERR\_free\_strings() return no values.

# SEE ALSO

[err(3)](http://man.he.net/man3/err), [ERR\_error\_string(3)](http://man.he.net/man3/ERR_error_string)

# HISTORY

The ERR\_load\_crypto\_strings(), SSL\_load\_error\_strings(), and
ERR\_free\_strings() functions were deprecated in OpenSSL 1.1.0 by
OPENSSL\_init\_crypto() and OPENSSL\_init\_ssl().

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
