# NAME

TLSv1\_2\_method, TLSv1\_2\_server\_method, TLSv1\_2\_client\_method,
SSL\_CTX\_new, SSL\_CTX\_up\_ref, SSLv3\_method, SSLv3\_server\_method,
SSLv3\_client\_method, TLSv1\_method, TLSv1\_server\_method, TLSv1\_client\_method,
TLSv1\_1\_method, TLSv1\_1\_server\_method, TLSv1\_1\_client\_method, TLS\_method,
TLS\_server\_method, TLS\_client\_method, SSLv23\_method, SSLv23\_server\_method,
SSLv23\_client\_method, DTLS\_method, DTLS\_server\_method, DTLS\_client\_method,
DTLSv1\_method, DTLSv1\_server\_method, DTLSv1\_client\_method,
DTLSv1\_2\_method, DTLSv1\_2\_server\_method, DTLSv1\_2\_client\_method
\- create a new SSL\_CTX object as framework for TLS/SSL or DTLS enabled
functions

# SYNOPSIS

    #include <openssl/ssl.h>

    SSL_CTX *SSL_CTX_new(const SSL_METHOD *method);
    int SSL_CTX_up_ref(SSL_CTX *ctx);

    const SSL_METHOD *TLS_method(void);
    const SSL_METHOD *TLS_server_method(void);
    const SSL_METHOD *TLS_client_method(void);

    const SSL_METHOD *SSLv23_method(void);
    const SSL_METHOD *SSLv23_server_method(void);
    const SSL_METHOD *SSLv23_client_method(void);

    #ifndef OPENSSL_NO_SSL3_METHOD
    const SSL_METHOD *SSLv3_method(void);
    const SSL_METHOD *SSLv3_server_method(void);
    const SSL_METHOD *SSLv3_client_method(void);
    #endif

    #ifndef OPENSSL_NO_TLS1_METHOD
    const SSL_METHOD *TLSv1_method(void);
    const SSL_METHOD *TLSv1_server_method(void);
    const SSL_METHOD *TLSv1_client_method(void);
    #endif

    #ifndef OPENSSL_NO_TLS1_1_METHOD
    const SSL_METHOD *TLSv1_1_method(void);
    const SSL_METHOD *TLSv1_1_server_method(void);
    const SSL_METHOD *TLSv1_1_client_method(void);
    #endif

    #ifndef OPENSSL_NO_TLS1_2_METHOD
    const SSL_METHOD *TLSv1_2_method(void);
    const SSL_METHOD *TLSv1_2_server_method(void);
    const SSL_METHOD *TLSv1_2_client_method(void);
    #endif

    const SSL_METHOD *DTLS_method(void);
    const SSL_METHOD *DTLS_server_method(void);
    const SSL_METHOD *DTLS_client_method(void);

    #ifndef OPENSSL_NO_DTLS1_METHOD
    const SSL_METHOD *DTLSv1_method(void);
    const SSL_METHOD *DTLSv1_server_method(void);
    const SSL_METHOD *DTLSv1_client_method(void);
    #endif

    #ifndef OPENSSL_NO_DTLS1_2_METHOD
    const SSL_METHOD *DTLSv1_2_method(void);
    const SSL_METHOD *DTLSv1_2_server_method(void);
    const SSL_METHOD *DTLSv1_2_client_method(void);
    #endif

# DESCRIPTION

SSL\_CTX\_new() creates a new **SSL\_CTX** object as framework to
establish TLS/SSL or DTLS enabled connections. An **SSL\_CTX** object is
reference counted. Creating an **SSL\_CTX** object for the first time increments
the reference count. Freeing it (using SSL\_CTX\_free) decrements it. When the
reference count drops to zero, any memory or resources allocated to the
**SSL\_CTX** object are freed. SSL\_CTX\_up\_ref() increments the reference count for
an existing **SSL\_CTX** structure.

# NOTES

The SSL\_CTX object uses **method** as connection method.
The methods exist in a generic type (for client and server use), a server only
type, and a client only type.
**method** can be of the following types:

- TLS\_method(), TLS\_server\_method(), TLS\_client\_method()

    These are the general-purpose _version-flexible_ SSL/TLS methods.
    The actual protocol version used will be negotiated to the highest version
    mutually supported by the client and the server.
    The supported protocols are SSLv3, TLSv1, TLSv1.1 and TLSv1.2.
    Applications should use these methods, and avoid the version-specific
    methods described below.

- SSLv23\_method(), SSLv23\_server\_method(), SSLv23\_client\_method()

    Use of these functions is deprecated. They have been replaced with the above
    TLS\_method(), TLS\_server\_method() and TLS\_client\_method() respectively. New
    code should use those functions instead.

- TLSv1\_2\_method(), TLSv1\_2\_server\_method(), TLSv1\_2\_client\_method()

    A TLS/SSL connection established with these methods will only understand the
    TLSv1.2 protocol.

- TLSv1\_1\_method(), TLSv1\_1\_server\_method(), TLSv1\_1\_client\_method()

    A TLS/SSL connection established with these methods will only understand the
    TLSv1.1 protocol.

- TLSv1\_method(), TLSv1\_server\_method(), TLSv1\_client\_method()

    A TLS/SSL connection established with these methods will only understand the
    TLSv1 protocol.

- SSLv3\_method(), SSLv3\_server\_method(), SSLv3\_client\_method()

    A TLS/SSL connection established with these methods will only understand the
    SSLv3 protocol.
    The SSLv3 protocol is deprecated and should not be used.

- DTLS\_method(), DTLS\_server\_method(), DTLS\_client\_method()

    These are the version-flexible DTLS methods.
    Currently supported protocols are DTLS 1.0 and DTLS 1.2.

- DTLSv1\_2\_method(), DTLSv1\_2\_server\_method(), DTLSv1\_2\_client\_method()

    These are the version-specific methods for DTLSv1.2.

- DTLSv1\_method(), DTLSv1\_server\_method(), DTLSv1\_client\_method()

    These are the version-specific methods for DTLSv1.

SSL\_CTX\_new() initializes the list of ciphers, the session cache setting, the
callbacks, the keys and certificates and the options to their default values.

TLS\_method(), TLS\_server\_method(), TLS\_client\_method(), DTLS\_method(),
DTLS\_server\_method() and DTLS\_client\_method() are the _version-flexible_
methods.
All other methods only support one specific protocol version.
Use the _version-flexible_ methods instead of the version specific methods.

If you want to limit the supported protocols for the version flexible
methods you can use [SSL\_CTX\_set\_min\_proto\_version(3)](http://man.he.net/man3/SSL_CTX_set_min_proto_version),
[SSL\_set\_min\_proto\_version(3)](http://man.he.net/man3/SSL_set_min_proto_version), [SSL\_CTX\_set\_max\_proto\_version(3)](http://man.he.net/man3/SSL_CTX_set_max_proto_version) and
[SSL\_set\_max\_proto\_version(3)](http://man.he.net/man3/SSL_set_max_proto_version) functions.
Using these functions it is possible to choose e.g. TLS\_server\_method()
and be able to negotiate with all possible clients, but to only
allow newer protocols like TLS 1.0, TLS 1.1 or TLS 1.2.

The list of protocols available can also be limited using the
**SSL\_OP\_NO\_SSLv3**, **SSL\_OP\_NO\_TLSv1**, **SSL\_OP\_NO\_TLSv1\_1** and
**SSL\_OP\_NO\_TLSv1\_2** options of the [SSL\_CTX\_set\_options(3)](http://man.he.net/man3/SSL_CTX_set_options) or
[SSL\_set\_options(3)](http://man.he.net/man3/SSL_set_options) functions, but this approach is not recommended.
Clients should avoid creating "holes" in the set of protocols they support.
When disabling a protocol, make sure that you also disable either all previous
or all subsequent protocol versions.
In clients, when a protocol version is disabled without disabling _all_
previous protocol versions, the effect is to also disable all subsequent
protocol versions.

The SSLv3 protocol is deprecated and should generally not be used.
Applications should typically use [SSL\_CTX\_set\_min\_proto\_version(3)](http://man.he.net/man3/SSL_CTX_set_min_proto_version) to set
the minimum protocol to at least **TLS1\_VERSION**.

# RETURN VALUES

The following return values can occur:

- NULL

    The creation of a new SSL\_CTX object failed. Check the error stack to find out
    the reason.

- Pointer to an SSL\_CTX object

    The return value points to an allocated SSL\_CTX object.

    SSL\_CTX\_up\_ref() returns 1 for success and 0 for failure.

# HISTORY

Support for SSLv2 and the corresponding SSLv2\_method(),
SSLv2\_server\_method() and SSLv2\_client\_method() functions where
removed in OpenSSL 1.1.0.

SSLv23\_method(), SSLv23\_server\_method() and SSLv23\_client\_method()
were deprecated and the preferred TLS\_method(), TLS\_server\_method()
and TLS\_client\_method() functions were introduced in OpenSSL 1.1.0.

All version-specific methods were deprecated in OpenSSL 1.1.0.

# SEE ALSO

[SSL\_CTX\_set\_options(3)](http://man.he.net/man3/SSL_CTX_set_options), [SSL\_CTX\_free(3)](http://man.he.net/man3/SSL_CTX_free), [SSL\_accept(3)](http://man.he.net/man3/SSL_accept),
[SSL\_CTX\_set\_min\_proto\_version(3)](http://man.he.net/man3/SSL_CTX_set_min_proto_version), [ssl(3)](http://man.he.net/man3/ssl), [SSL\_set\_connect\_state(3)](http://man.he.net/man3/SSL_set_connect_state)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
