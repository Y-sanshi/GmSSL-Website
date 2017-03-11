# NAME

SSL\_SESSION\_get0\_id\_context - get the SSL ID context associated with a session

# SYNOPSIS

    #include <openssl/ssl.h>

    const unsigned char *SSL_SESSION_get0_id_context(const SSL_SESSION *s,
                                                     unsigned int *len)

# DESCRIPTION

SSL\_SESSION\_get0\_id\_context() returns the ID context associated with
the SSL/TLS session **s**. The length of the ID context is written to
**\*len** if **len** is not NULL.

The value returned is a pointer to an object maintained within **s** and
should not be released.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_set\_session\_id\_context(3)](http://man.he.net/man3/SSL_set_session_id_context)

# HISTORY

SSL\_SESSION\_get0\_id\_context() was first added to OpenSSL 1.1.0

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
