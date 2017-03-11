# NAME

SSL\_SESSION\_set1\_id - set the SSL session ID

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_SESSION_set1_id(SSL_SESSION *s, const unsigned char *sid,
                            unsigned int sid_len);

# DESCRIPTION

SSL\_SESSION\_set1\_id() sets the the session ID for the **ssl** SSL/TLS session
to **sid** of length **sid\_len**.

# RETURN VALUES

SSL\_SESSION\_set1\_id() returns 1 for success and 0 for failure, for example
if the supplied session ID length exceeds **SSL\_MAX\_SSL\_SESSION\_ID\_LENGTH**.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl)

# HISTORY

SSL\_SESSION\_set1\_id() was first added to OpenSSL 1.1.0

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
