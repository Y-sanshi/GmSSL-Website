# NAME

SSL\_CTX\_set\_security\_level, SSL\_set\_security\_level, SSL\_CTX\_get\_security\_level, SSL\_get\_security\_level, SSL\_CTX\_set\_security\_callback, SSL\_set\_security\_callback, SSL\_CTX\_get\_security\_callback, SSL\_get\_security\_callback, SSL\_CTX\_set0\_security\_ex\_data, SSL\_set0\_security\_ex\_data, SSL\_CTX\_get0\_security\_ex\_data, SSL\_get0\_security\_ex\_data - SSL/TLS security framework

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_CTX_set_security_level(SSL_CTX *ctx, int level);
    void SSL_set_security_level(SSL *s, int level);

    int SSL_CTX_get_security_level(const SSL_CTX *ctx);
    int SSL_get_security_level(const SSL *s);

    void SSL_CTX_set_security_callback(SSL_CTX *ctx,
                   int (*cb)(SSL *s, SSL_CTX *ctx, int op, int bits, int nid,
                                                           void *other, void *ex));

    void SSL_set_security_callback(SSL *s,
                   int (*cb)(SSL *s, SSL_CTX *ctx, int op, int bits, int nid,
                                                           void *other, void *ex));

    int (*SSL_CTX_get_security_callback(const SSL_CTX *ctx))(SSL *s, SSL_CTX *ctx, int op, int bits, int nid, void *other, void *ex);
    int (*SSL_get_security_callback(const SSL *s))(SSL *s, SSL_CTX *ctx, int op, int bits, int nid, void *other, void *ex);

    void SSL_CTX_set0_security_ex_data(SSL_CTX *ctx, void *ex);
    void SSL_set0_security_ex_data(SSL *s, void *ex);

    void *SSL_CTX_get0_security_ex_data(const SSL_CTX *ctx);
    void *SSL_get0_security_ex_data(const SSL *s);

# DESCRIPTION

The functions SSL\_CTX\_set\_security\_level() and SSL\_set\_security\_level() set
the security level to **level**. If not set the library default security level
is used.

The functions SSL\_CTX\_get\_security\_level() and SSL\_get\_security\_level()
retrieve the current security level.

SSL\_CTX\_set\_security\_callback(), SSL\_set\_security\_callback(),
SSL\_CTX\_get\_security\_callback() and SSL\_get\_security\_callback() get or set
the security callback associated with **ctx** or **s**. If not set a default
security callback is used. The meaning of the parameters and the behaviour
of the default callbacks is described below.

SSL\_CTX\_set0\_security\_ex\_data(), SSL\_set0\_security\_ex\_data(),
SSL\_CTX\_get0\_security\_ex\_data() and SSL\_get0\_security\_ex\_data() set the
extra data pointer passed to the **ex** parameter of the callback. This
value is passed to the callback verbatim and can be set to any convenient
application specific value.

# DEFAULT CALLBACK BEHAVIOUR

If an application doesn't set its own security callback the default
callback is used. It is intended to provide sane defaults. The meaning
of each level is described below.

- **Level 0**

    Everything is permitted. This retains compatibility with previous versions of
    OpenSSL.

- **Level 1**

    The security level corresponds to a minimum of 80 bits of security. Any
    parameters offering below 80 bits of security are excluded. As a result RSA,
    DSA and DH keys shorter than 1024 bits and ECC keys shorter than 160 bits
    are prohibited. All export ciphersuites are prohibited since they all offer
    less than 80 bits of security. SSL version 2 is prohibited. Any ciphersuite
    using MD5 for the MAC is also prohibited.

- **Level 2**

    Security level set to 112 bits of security. As a result RSA, DSA and DH keys
    shorter than 2048 bits and ECC keys shorter than 224 bits are prohibited.
    In addition to the level 1 exclusions any ciphersuite using RC4 is also
    prohibited. SSL version 3 is also not allowed. Compression is disabled.

- **Level 3**

    Security level set to 128 bits of security. As a result RSA, DSA and DH keys
    shorter than 3072 bits and ECC keys shorter than 256 bits are prohibited.
    In addition to the level 2 exclusions ciphersuites not offering forward
    secrecy are prohibited. TLS versions below 1.1 are not permitted. Session
    tickets are disabled.

- **Level 4**

    Security level set to 192 bits of security. As a result RSA, DSA and DH keys
    shorter than 7680 bits and ECC keys shorter than 384 bits are prohibited.
    Ciphersuites using SHA1 for the MAC are prohibited. TLS versions below 1.2 are
    not permitted.

- **Level 5**

    Security level set to 256 bits of security. As a result RSA, DSA and DH keys
    shorter than 15360 bits and ECC keys shorter than 512 bits are prohibited.

# APPLICATION DEFINED SECURITY CALLBACKS

_Documentation to be provided._

# NOTES

**WARNING** at this time setting the security level higher than 1 for
general internet use is likely to cause **considerable** interoperability
issues and is not recommended. This is because the **SHA1** algorithm
is very widely used in certificates and will be rejected at levels
higher than 1 because it only offers 80 bits of security.

The default security level can be configured when OpenSSL is compiled by
setting **-DOPENSSL\_TLS\_SECURITY\_LEVEL=level**. If not set then 1 is used.

The security framework disables or reject parameters inconsistent with the
set security level. In the past this was difficult as applications had to set
a number of distinct parameters (supported ciphers, supported curves supported
signature algorithms) to achieve this end and some cases (DH parameter size
for example) could not be checked at all.

By setting an appropriate security level much of this complexity can be
avoided.

The bits of security limits affect all relevant parameters including
ciphersuite encryption algorithms, supported ECC curves, supported
signature algorithms, DH parameter sizes, certificate key sizes and
signature algorithms. This limit applies no matter what other custom
settings an application has set: so if the ciphersuite is set to **ALL**
then only ciphersuites consistent with the security level are permissible.

See SP800-57 for how the security limits are related to individual
algorithms.

Some security levels require large key sizes for non-ECC public key
algorithms which can severely degrade performance. For example 256 bits
of security requires the use of RSA keys of at least 15360 bits in size.

Some restrictions can be gracefully handled: for example ciphersuites
offering insufficient security are not sent by the client and will not
be selected by the server. Other restrictions such as the peer certificate
key size or the DH parameter size will abort the handshake with a fatal
alert.

Attempts to set certificates or parameters with insufficient security are
also blocked. For example trying to set a certificate using a 512 bit RSA
key using SSL\_CTX\_use\_certificate() at level 1. Applications which do not
check the return values for errors will misbehave: for example it might
appear that a certificate is not set at all because it had been rejected.

# HISTORY

These functions were first added to OpenSSL 1.1.0

# COPYRIGHT

Copyright 2014-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
