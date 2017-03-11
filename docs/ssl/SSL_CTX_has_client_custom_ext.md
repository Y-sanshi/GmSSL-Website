# NAME

SSL\_CTX\_has\_client\_custom\_ext - check whether a handler exists for a particular
client extension type

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CTX_has_client_custom_ext(const SSL_CTX *ctx, unsigned int ext_type);

# DESCRIPTION

SSL\_CTX\_has\_client\_custom\_ext() checks whether a handler has been set for a
client extension of type **ext\_type** using SSL\_CTX\_add\_client\_custom\_ext().

# RETURN VALUES

Returns 1 if a handler has been set, 0 otherwise.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_add\_client\_custom\_ext(3)](http://man.he.net/man3/SSL_CTX_add_client_custom_ext)

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
