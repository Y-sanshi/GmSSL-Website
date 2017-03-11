# NAME

SSL\_CTX\_set\_cert\_store, SSL\_CTX\_get\_cert\_store - manipulate X509 certificate verification storage

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_CTX_set_cert_store(SSL_CTX *ctx, X509_STORE *store);
    X509_STORE *SSL_CTX_get_cert_store(const SSL_CTX *ctx);

# DESCRIPTION

SSL\_CTX\_set\_cert\_store() sets/replaces the certificate verification storage
of **ctx** to/with **store**. If another X509\_STORE object is currently
set in **ctx**, it will be X509\_STORE\_free()ed.

SSL\_CTX\_get\_cert\_store() returns a pointer to the current certificate
verification storage.

# NOTES

In order to verify the certificates presented by the peer, trusted CA
certificates must be accessed. These CA certificates are made available
via lookup methods, handled inside the X509\_STORE. From the X509\_STORE
the X509\_STORE\_CTX used when verifying certificates is created.

Typically the trusted certificate store is handled indirectly via using
[SSL\_CTX\_load\_verify\_locations(3)](http://man.he.net/man3/SSL_CTX_load_verify_locations).
Using the SSL\_CTX\_set\_cert\_store() and SSL\_CTX\_get\_cert\_store() functions
it is possible to manipulate the X509\_STORE object beyond the
[SSL\_CTX\_load\_verify\_locations(3)](http://man.he.net/man3/SSL_CTX_load_verify_locations)
call.

Currently no detailed documentation on how to use the X509\_STORE
object is available. Not all members of the X509\_STORE are used when
the verification takes place. So will e.g. the verify\_callback() be
overridden with the verify\_callback() set via the
[SSL\_CTX\_set\_verify(3)](http://man.he.net/man3/SSL_CTX_set_verify) family of functions.
This document must therefore be updated when documentation about the
X509\_STORE object and its handling becomes available.

# RESTRICTIONS

The X509\_STORE structure used by an SSL\_CTX is used for verifying peer
certificates and building certificate chains, it is also shared by
every child SSL structure. Applications wanting finer control can use
functions such as SSL\_CTX\_set1\_verify\_cert\_store() instead.

# RETURN VALUES

SSL\_CTX\_set\_cert\_store() does not return diagnostic output.

SSL\_CTX\_get\_cert\_store() returns the current setting.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_load\_verify\_locations(3)](http://man.he.net/man3/SSL_CTX_load_verify_locations),
[SSL\_CTX\_set\_verify(3)](http://man.he.net/man3/SSL_CTX_set_verify)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
