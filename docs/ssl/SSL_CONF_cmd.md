# NAME

SSL\_CONF\_cmd\_value\_type, SSL\_CONF\_finish,
SSL\_CONF\_cmd - send configuration command

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CONF_cmd(SSL_CONF_CTX *cctx, const char *cmd, const char *value);
    int SSL_CONF_cmd_value_type(SSL_CONF_CTX *cctx, const char *cmd);
    int SSL_CONF_finish(SSL_CONF_CTX *cctx);

# DESCRIPTION

The function SSL\_CONF\_cmd() performs configuration operation **cmd** with
optional parameter **value** on **ctx**. Its purpose is to simplify application
configuration of **SSL\_CTX** or **SSL** structures by providing a common
framework for command line options or configuration files.

SSL\_CONF\_cmd\_value\_type() returns the type of value that **cmd** refers to.

The function SSL\_CONF\_finish() must be called after all configuration
operations have been completed. It is used to finalise any operations
or to process defaults.

# SUPPORTED COMMAND LINE COMMANDS

Currently supported **cmd** names for command lines (i.e. when the
flag **SSL\_CONF\_CMDLINE** is set) are listed below. Note: all **cmd** names
are case sensitive. Unless otherwise stated commands can be used by
both clients and servers and the **value** parameter is not used. The default
prefix for command line commands is **-** and that is reflected below.

- **-sigalgs**

    This sets the supported signature algorithms for TLS v1.2. For clients this
    value is used directly for the supported signature algorithms extension. For
    servers it is used to determine which signature algorithms to support.

    The **value** argument should be a colon separated list of signature algorithms
    in order of decreasing preference of the form **algorithm+hash**. **algorithm**
    is one of **RSA**, **DSA** or **ECDSA** and **hash** is a supported algorithm
    OID short name such as **SHA1**, **SHA224**, **SHA256**, **SHA384** of **SHA512**.
    Note: algorithm and hash names are case sensitive.

    If this option is not set then all signature algorithms supported by the
    OpenSSL library are permissible.

- **-client\_sigalgs**

    This sets the supported signature algorithms associated with client
    authentication for TLS v1.2. For servers the value is used in the supported
    signature algorithms field of a certificate request. For clients it is
    used to determine which signature algorithm to with the client certificate.
    If a server does not request a certificate this option has no effect.

    The syntax of **value** is identical to **-sigalgs**. If not set then
    the value set for **-sigalgs** will be used instead.

- **-curves**

    This sets the supported elliptic curves. For clients the curves are
    sent using the supported curves extension. For servers it is used
    to determine which curve to use. This setting affects curves used for both
    signatures and key exchange, if applicable.

    The **value** argument is a colon separated list of curves. The curve can be
    either the **NIST** name (e.g. **P-256**) or an OpenSSL OID name (e.g
    **prime256v1**). Curve names are case sensitive.

- **-named\_curve**

    This sets the temporary curve used for ephemeral ECDH modes. Only used by
    servers

    The **value** argument is a curve name or the special value **auto** which
    picks an appropriate curve based on client and server preferences. The curve
    can be either the **NIST** name (e.g. **P-256**) or an OpenSSL OID name
    (e.g **prime256v1**). Curve names are case sensitive.

- **-cipher**

    Sets the cipher suite list to **value**. Note: syntax checking of **value** is
    currently not performed unless a **SSL** or **SSL\_CTX** structure is
    associated with **cctx**.

- **-cert**

    Attempts to use the file **value** as the certificate for the appropriate
    context. It currently uses SSL\_CTX\_use\_certificate\_chain\_file() if an **SSL\_CTX**
    structure is set or SSL\_use\_certificate\_file() with filetype PEM if an **SSL**
    structure is set. This option is only supported if certificate operations
    are permitted.

- **-key**

    Attempts to use the file **value** as the private key for the appropriate
    context. This option is only supported if certificate operations
    are permitted. Note: if no **-key** option is set then a private key is
    not loaded unless the flag **SSL\_CONF\_FLAG\_REQUIRE\_PRIVATE** is set.

- **-dhparam**

    Attempts to use the file **value** as the set of temporary DH parameters for
    the appropriate context. This option is only supported if certificate
    operations are permitted.

- **-min\_protocol**, **-max\_protocol**

    Sets the minimum and maximum supported protocol.
    Currently supported protocol values are **SSLv3**, **TLSv1**,
    **TLSv1.1**, **TLSv1.2** for TLS and **DTLSv1**, **DTLSv1.2** for DTLS,
    and **None** for no limit.
    If the either bound is not specified then only the other bound applies,
    if specified.
    To restrict the supported protocol versions use these commands rather
    than the deprecated alternative commands below.

- **-no\_ssl3**, **-no\_tls1**, **-no\_tls1\_1**, **-no\_tls1\_2**

    Disables protocol support for SSLv3, TLSv1.0, TLSv1.1 or TLSv1.2 by setting the
    corresponding options **SSL\_OP\_NO\_SSLv3**, **SSL\_OP\_NO\_TLSv1**, **SSL\_OP\_NO\_TLSv1\_1**
    and **SSL\_OP\_NO\_TLSv1\_2** respectively.
    These options are deprecated, instead use **-min\_protocol** and **-max\_protocol**.

- **-bugs**

    Various bug workarounds are set, same as setting **SSL\_OP\_ALL**.

- **-comp**

    Enables support for SSL/TLS compression, same as clearing
    **SSL\_OP\_NO\_COMPRESSION**.
    This command was introduced in OpenSSL 1.1.0.
    As of OpenSSL 1.1.0, compression is off by default.

- **-no\_comp**

    Disables support for SSL/TLS compression, same as setting
    **SSL\_OP\_NO\_COMPRESSION**.
    As of OpenSSL 1.1.0, compression is off by default.

- **-no\_ticket**

    Disables support for session tickets, same as setting **SSL\_OP\_NO\_TICKET**.

- **-serverpref**

    Use server and not client preference order when determining which cipher suite,
    signature algorithm or elliptic curve to use for an incoming connection.
    Equivalent to **SSL\_OP\_CIPHER\_SERVER\_PREFERENCE**. Only used by servers.

- **-no\_resumption\_on\_reneg**

    set SSL\_OP\_NO\_SESSION\_RESUMPTION\_ON\_RENEGOTIATION flag. Only used by servers.

- **-legacyrenegotiation**

    permits the use of unsafe legacy renegotiation. Equivalent to setting
    **SSL\_OP\_ALLOW\_UNSAFE\_LEGACY\_RENEGOTIATION**.

- **-legacy\_server\_connect**, **-no\_legacy\_server\_connect**

    permits or prohibits the use of unsafe legacy renegotiation for OpenSSL
    clients only. Equivalent to setting or clearing **SSL\_OP\_LEGACY\_SERVER\_CONNECT**.
    Set by default.

- **-strict**

    enables strict mode protocol handling. Equivalent to setting
    **SSL\_CERT\_FLAG\_TLS\_STRICT**.

# SUPPORTED CONFIGURATION FILE COMMANDS

Currently supported **cmd** names for configuration files (i.e. when the
flag **SSL\_CONF\_FLAG\_FILE** is set) are listed below. All configuration file
**cmd** names are case insensitive so **signaturealgorithms** is recognised
as well as **SignatureAlgorithms**. Unless otherwise stated the **value** names
are also case insensitive.

Note: the command prefix (if set) alters the recognised **cmd** values.

- **CipherString**

    Sets the cipher suite list to **value**. Note: syntax checking of **value** is
    currently not performed unless an **SSL** or **SSL\_CTX** structure is
    associated with **cctx**.

- **Certificate**

    Attempts to use the file **value** as the certificate for the appropriate
    context. It currently uses SSL\_CTX\_use\_certificate\_chain\_file() if an **SSL\_CTX**
    structure is set or SSL\_use\_certificate\_file() with filetype PEM if an **SSL**
    structure is set. This option is only supported if certificate operations
    are permitted.

- **PrivateKey**

    Attempts to use the file **value** as the private key for the appropriate
    context. This option is only supported if certificate operations
    are permitted. Note: if no **PrivateKey** option is set then a private key is
    not loaded unless the **SSL\_CONF\_FLAG\_REQUIRE\_PRIVATE** is set.

- **ChainCAFile**, **ChainCAPath**, **VerifyCAFile**, **VerifyCAPath**

    These options indicate a file or directory used for building certificate
    chains or verifying certificate chains. These options are only supported
    if certificate operations are permitted.

- **ServerInfoFile**

    Attempts to use the file **value** in the "serverinfo" extension using the
    function SSL\_CTX\_use\_serverinfo\_file.

- **DHParameters**

    Attempts to use the file **value** as the set of temporary DH parameters for
    the appropriate context. This option is only supported if certificate
    operations are permitted.

- **SignatureAlgorithms**

    This sets the supported signature algorithms for TLS v1.2. For clients this
    value is used directly for the supported signature algorithms extension. For
    servers it is used to determine which signature algorithms to support.

    The **value** argument should be a colon separated list of signature algorithms
    in order of decreasing preference of the form **algorithm+hash**. **algorithm**
    is one of **RSA**, **DSA** or **ECDSA** and **hash** is a supported algorithm
    OID short name such as **SHA1**, **SHA224**, **SHA256**, **SHA384** of **SHA512**.
    Note: algorithm and hash names are case sensitive.

    If this option is not set then all signature algorithms supported by the
    OpenSSL library are permissible.

- **ClientSignatureAlgorithms**

    This sets the supported signature algorithms associated with client
    authentication for TLS v1.2. For servers the value is used in the supported
    signature algorithms field of a certificate request. For clients it is
    used to determine which signature algorithm to with the client certificate.

    The syntax of **value** is identical to **SignatureAlgorithms**. If not set then
    the value set for **SignatureAlgorithms** will be used instead.

- **Curves**

    This sets the supported elliptic curves. For clients the curves are
    sent using the supported curves extension. For servers it is used
    to determine which curve to use. This setting affects curves used for both
    signatures and key exchange, if applicable.

    The **value** argument is a colon separated list of curves. The curve can be
    either the **NIST** name (e.g. **P-256**) or an OpenSSL OID name (e.g
    **prime256v1**). Curve names are case sensitive.

- **MinProtocol**

    This sets the minimum supported SSL, TLS or DTLS version.

    Currently supported protocol values are **SSLv3**, **TLSv1**, **TLSv1.1**,
    **TLSv1.2**, **DTLSv1** and **DTLSv1.2**.
    The value **None** will disable the limit.

- **MaxProtocol**

    This sets the maximum supported SSL, TLS or DTLS version.

    Currently supported protocol values are **SSLv3**, **TLSv1**, **TLSv1.1**,
    **TLSv1.2**, **DTLSv1** and **DTLSv1.2**.
    The value **None** will disable the limit.

- **Protocol**

    This can be used to enable or disable certain versions of the SSL,
    TLS or DTLS protocol.

    The **value** argument is a comma separated list of supported protocols
    to enable or disable.
    If a protocol is preceded by **-** that version is disabled.

    All protocol versions are enabled by default.
    You need to disable at least one protocol version for this setting have any
    effect.
    Only enabling some protocol versions does not disable the other protocol
    versions.

    Currently supported protocol values are **SSLv3**, **TLSv1**, **TLSv1.1**,
    **TLSv1.2**, **DTLSv1** and **DTLSv1.2**.
    The special value **ALL** refers to all supported versions.

    This can't enable protocols that are disabled using **MinProtocol**
    or **MaxProtocol**, but can disable protocols that are still allowed
    by them.

    The **Protocol** command is fragile and deprecated; do not use it.
    Use **MinProtocol** and **MaxProtocol** instead.
    If you do use **Protocol**, make sure that the resulting range of enabled
    protocols has no "holes", e.g. if TLS 1.0 and TLS 1.2 are both enabled, make
    sure to also leave TLS 1.1 enabled.

- **Options**

    The **value** argument is a comma separated list of various flags to set.
    If a flag string is preceded **-** it is disabled.
    See the [SSL\_CTX\_set\_options(3)](http://man.he.net/man3/SSL_CTX_set_options) function for more details of
    individual options.

    Each option is listed below. Where an operation is enabled by default
    the **-flag** syntax is needed to disable it.

    **SessionTicket**: session ticket support, enabled by default. Inverse of
    **SSL\_OP\_NO\_TICKET**: that is **-SessionTicket** is the same as setting
    **SSL\_OP\_NO\_TICKET**.

    **Compression**: SSL/TLS compression support, enabled by default. Inverse
    of **SSL\_OP\_NO\_COMPRESSION**.

    **EmptyFragments**: use empty fragments as a countermeasure against a
    SSL 3.0/TLS 1.0 protocol vulnerability affecting CBC ciphers. It
    is set by default. Inverse of **SSL\_OP\_DONT\_INSERT\_EMPTY\_FRAGMENTS**.

    **Bugs**: enable various bug workarounds. Same as **SSL\_OP\_ALL**.

    **DHSingle**: enable single use DH keys, set by default. Inverse of
    **SSL\_OP\_DH\_SINGLE**. Only used by servers.

    **ECDHSingle** enable single use ECDH keys, set by default. Inverse of
    **SSL\_OP\_ECDH\_SINGLE**. Only used by servers.

    **ServerPreference** use server and not client preference order when
    determining which cipher suite, signature algorithm or elliptic curve
    to use for an incoming connection.  Equivalent to
    **SSL\_OP\_CIPHER\_SERVER\_PREFERENCE**. Only used by servers.

    **NoResumptionOnRenegotiation** set
    **SSL\_OP\_NO\_SESSION\_RESUMPTION\_ON\_RENEGOTIATION** flag. Only used by servers.

    **UnsafeLegacyRenegotiation** permits the use of unsafe legacy renegotiation.
    Equivalent to **SSL\_OP\_ALLOW\_UNSAFE\_LEGACY\_RENEGOTIATION**.

    **UnsafeLegacyServerConnect** permits the use of unsafe legacy renegotiation
    for OpenSSL clients only. Equivalent to **SSL\_OP\_LEGACY\_SERVER\_CONNECT**.
    Set by default.

- **VerifyMode**

    The **value** argument is a comma separated list of flags to set.

    **Peer** enables peer verification: for clients only.

    **Request** requests but does not require a certificate from the client.
    Servers only.

    **Require** requests and requires a certificate from the client: an error
    occurs if the client does not present a certificate. Servers only.

    **Once** requests a certificate from a client only on the initial connection:
    not when renegotiating. Servers only.

- **ClientCAFile**, **ClientCAPath**

    A file or directory of certificates in PEM format whose names are used as the
    set of acceptable names for client CAs. Servers only. This option is only
    supported if certificate operations are permitted.

# SUPPORTED COMMAND TYPES

The function SSL\_CONF\_cmd\_value\_type() currently returns one of the following
types:

- **SSL\_CONF\_TYPE\_UNKNOWN**

    The **cmd** string is unrecognised, this return value can be use to flag
    syntax errors.

- **SSL\_CONF\_TYPE\_STRING**

    The value is a string without any specific structure.

- **SSL\_CONF\_TYPE\_FILE**

    The value is a file name.

- **SSL\_CONF\_TYPE\_DIR**

    The value is a directory name.

- **SSL\_CONF\_TYPE\_NONE**

    The value string is not used e.g. a command line option which doesn't take an
    argument.

# NOTES

The order of operations is significant. This can be used to set either defaults
or values which cannot be overridden. For example if an application calls:

    SSL_CONF_cmd(ctx, "Protocol", "-SSLv3");
    SSL_CONF_cmd(ctx, userparam, uservalue);

it will disable SSLv3 support by default but the user can override it. If
however the call sequence is:

    SSL_CONF_cmd(ctx, userparam, uservalue);
    SSL_CONF_cmd(ctx, "Protocol", "-SSLv3");

SSLv3 is **always** disabled and attempt to override this by the user are
ignored.

By checking the return code of SSL\_CTX\_cmd() it is possible to query if a
given **cmd** is recognised, this is useful is SSL\_CTX\_cmd() values are
mixed with additional application specific operations.

For example an application might call SSL\_CTX\_cmd() and if it returns
\-2 (unrecognised command) continue with processing of application specific
commands.

Applications can also use SSL\_CTX\_cmd() to process command lines though the
utility function SSL\_CTX\_cmd\_argv() is normally used instead. One way
to do this is to set the prefix to an appropriate value using
SSL\_CONF\_CTX\_set1\_prefix(), pass the current argument to **cmd** and the
following argument to **value** (which may be NULL).

In this case if the return value is positive then it is used to skip that
number of arguments as they have been processed by SSL\_CTX\_cmd(). If -2 is
returned then **cmd** is not recognised and application specific arguments
can be checked instead. If -3 is returned a required argument is missing
and an error is indicated. If 0 is returned some other error occurred and
this can be reported back to the user.

The function SSL\_CONF\_cmd\_value\_type() can be used by applications to
check for the existence of a command or to perform additional syntax
checking or translation of the command value. For example if the return
value is **SSL\_CONF\_TYPE\_FILE** an application could translate a relative
pathname to an absolute pathname.

# EXAMPLES

Set supported signature algorithms:

    SSL_CONF_cmd(ctx, "SignatureAlgorithms", "ECDSA+SHA256:RSA+SHA256:DSA+SHA256");

There are various ways to select the supported protocols.

This set the minimum protocol version to TLSv1, and so disables SSLv3.
This is the recommended way to disable protocols.

    SSL_CONF_cmd(ctx, "MinProtocol", "TLSv1");

The following also disables SSLv3:

    SSL_CONF_cmd(ctx, "Protocol", "-SSLv3");

The following will first enable all protocols, and then disable
SSLv3.
If no protocol versions were disabled before this has the same effect as
"-SSLv3", but if some versions were disables this will re-enable them before
disabling SSLv3.

    SSL_CONF_cmd(ctx, "Protocol", "ALL,-SSLv3");

Only enable TLSv1.2:

    SSL_CONF_cmd(ctx, "MinProtocol", "TLSv1.2");
    SSL_CONF_cmd(ctx, "MaxProtocol", "TLSv1.2");

This also only enables TLSv1.2:

    SSL_CONF_cmd(ctx, "Protocol", "-ALL,TLSv1.2");

Disable TLS session tickets:

    SSL_CONF_cmd(ctx, "Options", "-SessionTicket");

Enable compression:

    SSL_CONF_cmd(ctx, "Options", "Compression");

Set supported curves to P-256, P-384:

    SSL_CONF_cmd(ctx, "Curves", "P-256:P-384");

Set automatic support for any elliptic curve for key exchange:

    SSL_CONF_cmd(ctx, "ECDHParameters", "Automatic");

# RETURN VALUES

SSL\_CONF\_cmd() returns 1 if the value of **cmd** is recognised and **value** is
**NOT** used and 2 if both **cmd** and **value** are used. In other words it
returns the number of arguments processed. This is useful when processing
command lines.

A return value of -2 means **cmd** is not recognised.

A return value of -3 means **cmd** is recognised and the command requires a
value but **value** is NULL.

A return code of 0 indicates that both **cmd** and **value** are valid but an
error occurred attempting to perform the operation: for example due to an
error in the syntax of **value** in this case the error queue may provide
additional information.

SSL\_CONF\_finish() returns 1 for success and 0 for failure.

# SEE ALSO

[SSL\_CONF\_CTX\_new(3)](http://man.he.net/man3/SSL_CONF_CTX_new),
[SSL\_CONF\_CTX\_set\_flags(3)](http://man.he.net/man3/SSL_CONF_CTX_set_flags),
[SSL\_CONF\_CTX\_set1\_prefix(3)](http://man.he.net/man3/SSL_CONF_CTX_set1_prefix),
[SSL\_CONF\_CTX\_set\_ssl\_ctx(3)](http://man.he.net/man3/SSL_CONF_CTX_set_ssl_ctx),
[SSL\_CONF\_cmd\_argv(3)](http://man.he.net/man3/SSL_CONF_cmd_argv),
[SSL\_CTX\_set\_options(3)](http://man.he.net/man3/SSL_CTX_set_options)

# HISTORY

SSL\_CONF\_cmd() was first added to OpenSSL 1.0.2

**SSL\_OP\_NO\_SSL2** doesn't have effect since 1.1.0, but the macro is retained
for backwards compatibility.

**SSL\_CONF\_TYPE\_NONE** was first added to OpenSSL 1.1.0. In earlier versions of
OpenSSL passing a command which didn't take an argument would return
**SSL\_CONF\_TYPE\_UNKNOWN**.

**MinProtocol** and **MaxProtocol** where added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2012-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
