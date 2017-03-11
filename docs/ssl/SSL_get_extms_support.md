# NAME

SSL\_get\_extms\_support - extended master secret support

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_get_extms_support(SSL *ssl);

# DESCRIPTION

SSL\_get\_extms\_support() indicates whether the current session used extended
master secret.

This function is implemented as a macro.

# RETURN VALUES

SSL\_get\_extms\_support() returns 1 if the current session used extended
master secret, 0 if it did not and -1 if a handshake is currently in
progress i.e. it is not possible to determine if extended master secret
was used.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
