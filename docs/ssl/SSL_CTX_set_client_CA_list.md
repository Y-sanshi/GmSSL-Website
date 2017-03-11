# NAME

SSL\_CTX\_set\_client\_CA\_list, SSL\_set\_client\_CA\_list, SSL\_CTX\_add\_client\_CA,
SSL\_add\_client\_CA - set list of CAs sent to the client when requesting a
client certificate

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_CTX_set_client_CA_list(SSL_CTX *ctx, STACK_OF(X509_NAME) *list);
    void SSL_set_client_CA_list(SSL *s, STACK_OF(X509_NAME) *list);
    int SSL_CTX_add_client_CA(SSL_CTX *ctx, X509 *cacert);
    int SSL_add_client_CA(SSL *ssl, X509 *cacert);

# DESCRIPTION

SSL\_CTX\_set\_client\_CA\_list() sets the **list** of CAs sent to the client when
requesting a client certificate for **ctx**.

SSL\_set\_client\_CA\_list() sets the **list** of CAs sent to the client when
requesting a client certificate for the chosen **ssl**, overriding the
setting valid for **ssl**'s SSL\_CTX object.

SSL\_CTX\_add\_client\_CA() adds the CA name extracted from **cacert** to the
list of CAs sent to the client when requesting a client certificate for
**ctx**.

SSL\_add\_client\_CA() adds the CA name extracted from **cacert** to the
list of CAs sent to the client when requesting a client certificate for
the chosen **ssl**, overriding the setting valid for **ssl**'s SSL\_CTX object.

# NOTES

When a TLS/SSL server requests a client certificate (see
**SSL\_CTX\_set\_verify(3)**), it sends a list of CAs, for which
it will accept certificates, to the client.

This list must explicitly be set using SSL\_CTX\_set\_client\_CA\_list() for
**ctx** and SSL\_set\_client\_CA\_list() for the specific **ssl**. The list
specified overrides the previous setting. The CAs listed do not become
trusted (**list** only contains the names, not the complete certificates); use
[SSL\_CTX\_load\_verify\_locations(3)](http://man.he.net/man3/SSL_CTX_load_verify_locations)
to additionally load them for verification.

If the list of acceptable CAs is compiled in a file, the
[SSL\_load\_client\_CA\_file(3)](http://man.he.net/man3/SSL_load_client_CA_file)
function can be used to help importing the necessary data.

SSL\_CTX\_add\_client\_CA() and SSL\_add\_client\_CA() can be used to add additional
items the list of client CAs. If no list was specified before using
SSL\_CTX\_set\_client\_CA\_list() or SSL\_set\_client\_CA\_list(), a new client
CA list for **ctx** or **ssl** (as appropriate) is opened.

These functions are only useful for TLS/SSL servers.

# RETURN VALUES

SSL\_CTX\_set\_client\_CA\_list() and SSL\_set\_client\_CA\_list() do not return
diagnostic information.

SSL\_CTX\_add\_client\_CA() and SSL\_add\_client\_CA() have the following return
values:

- 0

    A failure while manipulating the STACK\_OF(X509\_NAME) object occurred or
    the X509\_NAME could not be extracted from **cacert**. Check the error stack
    to find out the reason.

- 1

    The operation succeeded.

# EXAMPLES

Scan all certificates in **CAfile** and list them as acceptable CAs:

    SSL_CTX_set_client_CA_list(ctx, SSL_load_client_CA_file(CAfile));

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_get\_client\_CA\_list(3)](http://man.he.net/man3/SSL_get_client_CA_list),
[SSL\_load\_client\_CA\_file(3)](http://man.he.net/man3/SSL_load_client_CA_file),
[SSL\_CTX\_load\_verify\_locations(3)](http://man.he.net/man3/SSL_CTX_load_verify_locations)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
