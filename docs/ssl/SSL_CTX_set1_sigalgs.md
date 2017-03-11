# NAME

SSL\_CTX\_set1\_sigalgs, SSL\_set1\_sigalgs, SSL\_CTX\_set1\_sigalgs\_list,
SSL\_set1\_sigalgs\_list, SSL\_CTX\_set1\_client\_sigalgs,
SSL\_set1\_client\_sigalgs, SSL\_CTX\_set1\_client\_sigalgs\_list,
SSL\_set1\_client\_sigalgs\_list - set supported signature algorithms

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_CTX_set1_sigalgs(SSL_CTX *ctx, const int *slist, long slistlen);
    long SSL_set1_sigalgs(SSL *ssl, const int *slist, long slistlen);
    long SSL_CTX_set1_sigalgs_list(SSL_CTX *ctx, const char *str);
    long SSL_set1_sigalgs_list(SSL *ssl, const char *str);

    long SSL_CTX_set1_client_sigalgs(SSL_CTX *ctx, const int *slist, long slistlen);
    long SSL_set1_client_sigalgs(SSL *ssl, const int *slist, long slistlen);
    long SSL_CTX_set1_client_sigalgs_list(SSL_CTX *ctx, const char *str);
    long SSL_set1_client_sigalgs_list(SSL *ssl, const char *str);

# DESCRIPTION

SSL\_CTX\_set1\_sigalgs() and SSL\_set1\_sigalgs() set the supported signature
algorithms for **ctx** or **ssl**. The array **slist** of length **slistlen**
must consist of pairs of NIDs corresponding to digest and public key
algorithms.

SSL\_CTX\_set1\_sigalgs\_list() and SSL\_set1\_sigalgs\_list() set the supported
signature algorithms for **ctx** or **ssl**. The **str** parameter
must be a null terminated string consisting or a colon separated list of
public key algorithms and digests separated by **+**.

SSL\_CTX\_set1\_client\_sigalgs(), SSL\_set1\_client\_sigalgs(),
SSL\_CTX\_set1\_client\_sigalgs\_list() and SSL\_set1\_client\_sigalgs\_list() set
signature algorithms related to client authentication, otherwise they are
identical to SSL\_CTX\_set1\_sigalgs(), SSL\_set1\_sigalgs(),
SSL\_CTX\_set1\_sigalgs\_list() and SSL\_set1\_sigalgs\_list().

All these functions are implemented as macros. The signature algorithm
parameter (integer array or string) is not freed: the application should
free it, if necessary.

# NOTES

If an application wishes to allow the setting of signature algorithms
as one of many user configurable options it should consider using the more
flexible SSL\_CONF API instead.

The signature algorithms set by a client are used directly in the supported
signature algorithm in the client hello message.

The supported signature algorithms set by a server are not sent to the
client but are used to determine the set of shared signature algorithms
and (if server preferences are set with SSL\_OP\_CIPHER\_SERVER\_PREFERENCE)
their order.

The client authentication signature algorithms set by a server are sent
in a certificate request message if client authentication is enabled,
otherwise they are unused.

Similarly client authentication signature algorithms set by a client are
used to determined the set of client authentication shared signature
algorithms.

Signature algorithms will neither be advertised nor used if the security level
prohibits them (for example SHA1 if the security level is 4 or more).

Currently the NID\_md5, NID\_sha1, NID\_sha224, NID\_sha256, NID\_sha384 and
NID\_sha512 digest NIDs are supported and the public key algorithm NIDs
EVP\_PKEY\_RSA, EVP\_PKEY\_DSA and EVP\_PKEY\_EC.

The short or long name values for digests can be used in a string (for
example "MD5", "SHA1", "SHA224", "SHA256", "SHA384", "SHA512") and
the public key algorithm strings "RSA", "DSA" or "ECDSA".

The use of MD5 as a digest is strongly discouraged due to security weaknesses.

# EXAMPLES

Set supported signature algorithms to SHA256 with ECDSA and SHA256 with RSA
using an array:

    const int slist[] = {NID_sha256, EVP_PKEY_EC, NID_sha256, EVP_PKEY_RSA};

    SSL_CTX_set1_sigalgs(ctx, slist, 4);

Set supported signature algorithms to SHA256 with ECDSA and SHA256 with RSA
using a string:

    SSL_CTX_set1_sigalgs_list(ctx, "ECDSA+SHA256:RSA+SHA256");

# RETURN VALUES

All these functions return 1 for success and 0 for failure.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_get\_shared\_sigalgs(3)](http://man.he.net/man3/SSL_get_shared_sigalgs),
[SSL\_CONF\_CTX\_new(3)](http://man.he.net/man3/SSL_CONF_CTX_new)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
