# NAME

SSL\_rstate\_string, SSL\_rstate\_string\_long - get textual description of state of an SSL object during read operation

# SYNOPSIS

    #include <openssl/ssl.h>

    const char *SSL_rstate_string(SSL *ssl);
    const char *SSL_rstate_string_long(SSL *ssl);

# DESCRIPTION

SSL\_rstate\_string() returns a 2 letter string indicating the current read state
of the SSL object **ssl**.

SSL\_rstate\_string\_long() returns a string indicating the current read state of
the SSL object **ssl**.

# NOTES

When performing a read operation, the SSL/TLS engine must parse the record,
consisting of header and body. When working in a blocking environment,
SSL\_rstate\_string\[\_long\]() should always return "RD"/"read done".

This function should only seldom be needed in applications.

# RETURN VALUES

SSL\_rstate\_string() and SSL\_rstate\_string\_long() can return the following
values:

- "RH"/"read header"

    The header of the record is being evaluated.

- "RB"/"read body"

    The body of the record is being evaluated.

- "RD"/"read done"

    The record has been completely processed.

- "unknown"/"unknown"

    The read state is unknown. This should never happen.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
