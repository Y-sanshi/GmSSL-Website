# NAME

SSL\_get\_psk\_identity, SSL\_get\_psk\_identity\_hint - get PSK client identity and hint

# SYNOPSIS

    #include <openssl/ssl.h>

    const char *SSL_get_psk_identity_hint(const SSL *ssl);
    const char *SSL_get_psk_identity(const SSL *ssl);

# DESCRIPTION

SSL\_get\_psk\_identity\_hint() is used to retrieve the PSK identity hint
used during the connection setup related to SSL object
**ssl**. Similarly, SSL\_get\_psk\_identity() is used to retrieve the PSK
identity used during the connection setup.

# RETURN VALUES

If non-**NULL**, SSL\_get\_psk\_identity\_hint() returns the PSK identity
hint and SSL\_get\_psk\_identity() returns the PSK identity. Both are
**NULL**-terminated. SSL\_get\_psk\_identity\_hint() may return **NULL** if
no PSK identity hint was used during the connection setup.

Note that the return value is valid only during the lifetime of the
SSL object **ssl**.

# COPYRIGHT

Copyright 2006-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).

Copyright 2005 Nokia.
