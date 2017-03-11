# NAME

SSL\_get\_current\_cipher, SSL\_get\_cipher\_name, SSL\_get\_cipher,
SSL\_get\_cipher\_bits, SSL\_get\_cipher\_version - get SSL\_CIPHER of a connection

# SYNOPSIS

    #include <openssl/ssl.h>

    SSL_CIPHER *SSL_get_current_cipher(const SSL *ssl);

    const char *SSL_get_cipher_name(const SSL *s);
    const char *SSL_get_cipher(const SSL *s);
    int SSL_get_cipher_bits(const SSL *s, int *np) \
    const char *SSL_get_cipher_version(const SSL *s);

# DESCRIPTION

SSL\_get\_current\_cipher() returns a pointer to an SSL\_CIPHER object containing
the description of the actually used cipher of a connection established with
the **ssl** object.
See [SSL\_CIPHER\_get\_name(3)](http://man.he.net/man3/SSL_CIPHER_get_name) for more details.

SSL\_get\_cipher\_name() obtains the
name of the currently used cipher.
SSL\_get\_cipher() is identical to SSL\_get\_cipher\_name().
SSL\_get\_cipher\_bits() is a
macro to obtain the number of secret/algorithm bits used and
SSL\_get\_cipher\_version() returns the protocol name.

# RETURN VALUES

SSL\_get\_current\_cipher() returns the cipher actually used, or NULL if
no session has been established.

# NOTES

These are implemented as macros.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_CIPHER\_get\_name(3)](http://man.he.net/man3/SSL_CIPHER_get_name)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
