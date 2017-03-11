# NAME

SSL\_CTX\_set\_options, SSL\_set\_options, SSL\_CTX\_clear\_options,
SSL\_clear\_options, SSL\_CTX\_get\_options, SSL\_get\_options,
SSL\_get\_secure\_renegotiation\_support - manipulate SSL options

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_CTX_set_options(SSL_CTX *ctx, long options);
    long SSL_set_options(SSL *ssl, long options);

    long SSL_CTX_clear_options(SSL_CTX *ctx, long options);
    long SSL_clear_options(SSL *ssl, long options);

    long SSL_CTX_get_options(SSL_CTX *ctx);
    long SSL_get_options(SSL *ssl);

    long SSL_get_secure_renegotiation_support(SSL *ssl);

# DESCRIPTION

SSL\_CTX\_set\_options() adds the options set via bitmask in **options** to **ctx**.
Options already set before are not cleared!

SSL\_set\_options() adds the options set via bitmask in **options** to **ssl**.
Options already set before are not cleared!

SSL\_CTX\_clear\_options() clears the options set via bitmask in **options**
to **ctx**.

SSL\_clear\_options() clears the options set via bitmask in **options** to **ssl**.

SSL\_CTX\_get\_options() returns the options set for **ctx**.

SSL\_get\_options() returns the options set for **ssl**.

SSL\_get\_secure\_renegotiation\_support() indicates whether the peer supports
secure renegotiation.
Note, this is implemented via a macro.

# NOTES

The behaviour of the SSL library can be changed by setting several options.
The options are coded as bitmasks and can be combined by a bitwise **or**
operation (|).

SSL\_CTX\_set\_options() and SSL\_set\_options() affect the (external)
protocol behaviour of the SSL library. The (internal) behaviour of
the API can be changed by using the similar
[SSL\_CTX\_set\_mode(3)](http://man.he.net/man3/SSL_CTX_set_mode) and SSL\_set\_mode() functions.

During a handshake, the option settings of the SSL object are used. When
a new SSL object is created from a context using SSL\_new(), the current
option setting is copied. Changes to **ctx** do not affect already created
SSL objects. SSL\_clear() does not affect the settings.

The following **bug workaround** options are available:

- SSL\_OP\_SSLREF2\_REUSE\_CERT\_TYPE\_BUG

    ...

- SSL\_OP\_MICROSOFT\_BIG\_SSLV3\_BUFFER

    ...

- SSL\_OP\_SAFARI\_ECDHE\_ECDSA\_BUG

    Don't prefer ECDHE-ECDSA ciphers when the client appears to be Safari on OS X.
    OS X 10.8..10.8.3 has broken support for ECDHE-ECDSA ciphers.

- SSL\_OP\_SSLEAY\_080\_CLIENT\_DH\_BUG

    ...

- SSL\_OP\_TLS\_D5\_BUG

    ...

- SSL\_OP\_DONT\_INSERT\_EMPTY\_FRAGMENTS

    Disables a countermeasure against a SSL 3.0/TLS 1.0 protocol
    vulnerability affecting CBC ciphers, which cannot be handled by some
    broken SSL implementations.  This option has no effect for connections
    using other ciphers.

- SSL\_OP\_TLSEXT\_PADDING

    Adds a padding extension to ensure the ClientHello size is never between
    256 and 511 bytes in length. This is needed as a workaround for some
    implementations.

- SSL\_OP\_ALL

    All of the above bug workarounds.

It is usually safe to use **SSL\_OP\_ALL** to enable the bug workaround
options if compatibility with somewhat broken implementations is
desired.

The following **modifying** options are available:

- SSL\_OP\_TLS\_ROLLBACK\_BUG

    Disable version rollback attack detection.

    During the client key exchange, the client must send the same information
    about acceptable SSL/TLS protocol levels as during the first hello. Some
    clients violate this rule by adapting to the server's answer. (Example:
    the client sends a SSLv2 hello and accepts up to SSLv3.1=TLSv1, the server
    only understands up to SSLv3. In this case the client must still use the
    same SSLv3.1=TLSv1 announcement. Some clients step down to SSLv3 with respect
    to the server's answer and violate the version rollback protection.)

- SSL\_OP\_SINGLE\_DH\_USE

    Always create a new key when using temporary/ephemeral DH parameters
    (see [SSL\_CTX\_set\_tmp\_dh\_callback(3)](http://man.he.net/man3/SSL_CTX_set_tmp_dh_callback)).
    This option must be used to prevent small subgroup attacks, when
    the DH parameters were not generated using "strong" primes
    (e.g. when using DSA-parameters, see [dhparam(1)](http://man.he.net/man1/dhparam)).
    If "strong" primes were used, it is not strictly necessary to generate
    a new DH key during each handshake but it is also recommended.
    **SSL\_OP\_SINGLE\_DH\_USE** should therefore be enabled whenever
    temporary/ephemeral DH parameters are used.

- SSL\_OP\_EPHEMERAL\_RSA

    This option is no longer implemented and is treated as no op.

- SSL\_OP\_CIPHER\_SERVER\_PREFERENCE

    When choosing a cipher, use the server's preferences instead of the client
    preferences. When not set, the SSL server will always follow the clients
    preferences. When set, the SSL/TLS server will choose following its
    own preferences.

- SSL\_OP\_PKCS1\_CHECK\_1

    ...

- SSL\_OP\_PKCS1\_CHECK\_2

    ...

- SSL\_OP\_NO\_SSLv3, SSL\_OP\_NO\_TLSv1, SSL\_OP\_NO\_TLSv1\_1,
SSL\_OP\_NO\_TLSv1\_2, SSL\_OP\_NO\_DTLSv1, SSL\_OP\_NO\_DTLSv1\_2

    These options turn off the SSLv3, TLSv1, TLSv1.1 or TLSv1.2 protocol
    versions with TLS or the DTLSv1, DTLSv1.2 versions with DTLS,
    respectively.
    As of OpenSSL 1.1.0, these options are deprecated, use
    [SSL\_CTX\_set\_min\_proto\_version(3)](http://man.he.net/man3/SSL_CTX_set_min_proto_version) and
    [SSL\_CTX\_set\_max\_proto\_version(3)](http://man.he.net/man3/SSL_CTX_set_max_proto_version) instead.

- SSL\_OP\_NO\_SESSION\_RESUMPTION\_ON\_RENEGOTIATION

    When performing renegotiation as a server, always start a new session
    (i.e., session resumption requests are only accepted in the initial
    handshake). This option is not needed for clients.

- SSL\_OP\_NO\_TICKET

    Normally clients and servers will, where possible, transparently make use
    of RFC4507bis tickets for stateless session resumption.

    If this option is set this functionality is disabled and tickets will
    not be used by clients or servers.

- SSL\_OP\_ALLOW\_UNSAFE\_LEGACY\_RENEGOTIATION

    Allow legacy insecure renegotiation between OpenSSL and unpatched clients or
    servers. See the **SECURE RENEGOTIATION** section for more details.

- SSL\_OP\_LEGACY\_SERVER\_CONNECT

    Allow legacy insecure renegotiation between OpenSSL and unpatched servers
    **only**: this option is currently set by default. See the
    **SECURE RENEGOTIATION** section for more details.

# SECURE RENEGOTIATION

OpenSSL always attempts to use secure renegotiation as
described in RFC5746. This counters the prefix attack described in
CVE-2009-3555 and elsewhere.

This attack has far reaching consequences which application writers should be
aware of. In the description below an implementation supporting secure
renegotiation is referred to as _patched_. A server not supporting secure
renegotiation is referred to as _unpatched_.

The following sections describe the operations permitted by OpenSSL's secure
renegotiation implementation.

## Patched client and server

Connections and renegotiation are always permitted by OpenSSL implementations.

## Unpatched client and patched OpenSSL server

The initial connection succeeds but client renegotiation is denied by the
server with a **no\_renegotiation** warning alert if TLS v1.0 is used or a fatal
**handshake\_failure** alert in SSL v3.0.

If the patched OpenSSL server attempts to renegotiate a fatal
**handshake\_failure** alert is sent. This is because the server code may be
unaware of the unpatched nature of the client.

If the option **SSL\_OP\_ALLOW\_UNSAFE\_LEGACY\_RENEGOTIATION** is set then
renegotiation **always** succeeds.

## Patched OpenSSL client and unpatched server.

If the option **SSL\_OP\_LEGACY\_SERVER\_CONNECT** or
**SSL\_OP\_ALLOW\_UNSAFE\_LEGACY\_RENEGOTIATION** is set then initial connections
and renegotiation between patched OpenSSL clients and unpatched servers
succeeds. If neither option is set then initial connections to unpatched
servers will fail.

The option **SSL\_OP\_LEGACY\_SERVER\_CONNECT** is currently set by default even
though it has security implications: otherwise it would be impossible to
connect to unpatched servers (i.e. all of them initially) and this is clearly
not acceptable. Renegotiation is permitted because this does not add any
additional security issues: during an attack clients do not see any
renegotiations anyway.

As more servers become patched the option **SSL\_OP\_LEGACY\_SERVER\_CONNECT** will
**not** be set by default in a future version of OpenSSL.

OpenSSL client applications wishing to ensure they can connect to unpatched
servers should always **set** **SSL\_OP\_LEGACY\_SERVER\_CONNECT**

OpenSSL client applications that want to ensure they can **not** connect to
unpatched servers (and thus avoid any security issues) should always **clear**
**SSL\_OP\_LEGACY\_SERVER\_CONNECT** using SSL\_CTX\_clear\_options() or
SSL\_clear\_options().

The difference between the **SSL\_OP\_LEGACY\_SERVER\_CONNECT** and
**SSL\_OP\_ALLOW\_UNSAFE\_LEGACY\_RENEGOTIATION** options is that
**SSL\_OP\_LEGACY\_SERVER\_CONNECT** enables initial connections and secure
renegotiation between OpenSSL clients and unpatched servers **only**, while
**SSL\_OP\_ALLOW\_UNSAFE\_LEGACY\_RENEGOTIATION** allows initial connections
and renegotiation between OpenSSL and unpatched clients or servers.

# RETURN VALUES

SSL\_CTX\_set\_options() and SSL\_set\_options() return the new options bitmask
after adding **options**.

SSL\_CTX\_clear\_options() and SSL\_clear\_options() return the new options bitmask
after clearing **options**.

SSL\_CTX\_get\_options() and SSL\_get\_options() return the current bitmask.

SSL\_get\_secure\_renegotiation\_support() returns 1 is the peer supports
secure renegotiation and 0 if it does not.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_new(3)](http://man.he.net/man3/SSL_new), [SSL\_clear(3)](http://man.he.net/man3/SSL_clear),
[SSL\_CTX\_set\_tmp\_dh\_callback(3)](http://man.he.net/man3/SSL_CTX_set_tmp_dh_callback),
[SSL\_CTX\_set\_min\_proto\_version(3)](http://man.he.net/man3/SSL_CTX_set_min_proto_version),
[dhparam(1)](http://man.he.net/man1/dhparam)

# HISTORY

The attempt to always try to use secure renegotiation was added in
Openssl 0.9.8m.

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
