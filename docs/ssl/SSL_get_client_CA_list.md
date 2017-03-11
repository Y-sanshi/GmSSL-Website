# NAME

SSL\_get\_client\_CA\_list, SSL\_CTX\_get\_client\_CA\_list - get list of client CAs

# SYNOPSIS

    #include <openssl/ssl.h>

    STACK_OF(X509_NAME) *SSL_get_client_CA_list(const SSL *s);
    STACK_OF(X509_NAME) *SSL_CTX_get_client_CA_list(const SSL_CTX *ctx);

# DESCRIPTION

SSL\_CTX\_get\_client\_CA\_list() returns the list of client CAs explicitly set for
**ctx** using [SSL\_CTX\_set\_client\_CA\_list(3)](http://man.he.net/man3/SSL_CTX_set_client_CA_list).

SSL\_get\_client\_CA\_list() returns the list of client CAs explicitly
set for **ssl** using SSL\_set\_client\_CA\_list() or **ssl**'s SSL\_CTX object with
[SSL\_CTX\_set\_client\_CA\_list(3)](http://man.he.net/man3/SSL_CTX_set_client_CA_list), when in
server mode. In client mode, SSL\_get\_client\_CA\_list returns the list of
client CAs sent from the server, if any.

# RETURN VALUES

SSL\_CTX\_set\_client\_CA\_list() and SSL\_set\_client\_CA\_list() do not return
diagnostic information.

SSL\_CTX\_add\_client\_CA() and SSL\_add\_client\_CA() have the following return
values:

- STACK\_OF(X509\_NAMES)

    List of CA names explicitly set (for **ctx** or in server mode) or send
    by the server (client mode).

- NULL

    No client CA list was explicitly set (for **ctx** or in server mode) or
    the server did not send a list of CAs (client mode).

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_set\_client\_CA\_list(3)](http://man.he.net/man3/SSL_CTX_set_client_CA_list),
[SSL\_CTX\_set\_client\_cert\_cb(3)](http://man.he.net/man3/SSL_CTX_set_client_cert_cb)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
