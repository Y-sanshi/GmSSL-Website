# NAME

SSL\_SESSION\_get0\_cipher - retrieve the SSL cipher associated with a session

# SYNOPSIS

    #include <openssl/ssl.h>

    const SSL_CIPHER *SSL_SESSION_get0_cipher(const SSL_SESSSION *s);

# DESCRIPTION

SSL\_SESSION\_get0\_cipher() retrieves the cipher that was used by the
connection when the session was created, or NULL if it cannot be determined.

The value returned is a pointer to an object maintained within **s** and
should not be released.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[d2i\_SSL\_SESSION(3)](http://man.he.net/man3/d2i_SSL_SESSION),
[SSL\_SESSION\_get\_time(3)](http://man.he.net/man3/SSL_SESSION_get_time),
[SSL\_SESSION\_get0\_hostname(3)](http://man.he.net/man3/SSL_SESSION_get0_hostname),
[SSL\_SESSION\_free(3)](http://man.he.net/man3/SSL_SESSION_free)

# HISTORY

SSL\_SESSION\_get0\_cipher() was first added to OpenSSL 1.1.0

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
