# NAME

SSL\_check\_chain - check certificate chain suitability

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_check_chain(SSL *s, X509 *x, EVP_PKEY *pk, STACK_OF(X509) *chain);

# DESCRIPTION

SSL\_check\_chain() checks whether certificate **x**, private key **pk** and
certificate chain **chain** is suitable for use with the current session
**s**.

# RETURN VALUES

SSL\_check\_chain() returns a bitmap of flags indicating the validity of the
chain.

**CERT\_PKEY\_VALID**: the chain can be used with the current session.
If this flag is **not** set then the certificate will never be used even
if the application tries to set it because it is inconsistent with the
peer preferences.

**CERT\_PKEY\_SIGN**: the EE key can be used for signing.

**CERT\_PKEY\_EE\_SIGNATURE**: the signature algorithm of the EE certificate is
acceptable.

**CERT\_PKEY\_CA\_SIGNATURE**: the signature algorithms of all CA certificates
are acceptable.

**CERT\_PKEY\_EE\_PARAM**: the parameters of the end entity certificate are
acceptable (e.g. it is a supported curve).

**CERT\_PKEY\_CA\_PARAM**: the parameters of all CA certificates are acceptable.

**CERT\_PKEY\_EXPLICIT\_SIGN**: the end entity certificate algorithm
can be used explicitly for signing (i.e. it is mentioned in the signature
algorithms extension).

**CERT\_PKEY\_ISSUER\_NAME**: the issuer name is acceptable. This is only
meaningful for client authentication.

**CERT\_PKEY\_CERT\_TYPE**: the certificate type is acceptable. Only meaningful
for client authentication.

**CERT\_PKEY\_SUITEB**: chain is suitable for Suite B use.

# NOTES

SSL\_check\_chain() must be called in servers after a client hello message or in
clients after a certificate request message. It will typically be called
in the certificate callback.

An application wishing to support multiple certificate chains may call this
function on each chain in turn: starting with the one it considers the
most secure. It could then use the chain of the first set which returns
suitable flags.

As a minimum the flag **CERT\_PKEY\_VALID** must be set for a chain to be
usable. An application supporting multiple chains with different CA signature
algorithms may also wish to check **CERT\_PKEY\_CA\_SIGNATURE** too. If no
chain is suitable a server should fall back to the most secure chain which
sets **CERT\_PKEY\_VALID**.

The validity of a chain is determined by checking if it matches a supported
signature algorithm, supported curves and in the case of client authentication
certificate types and issuer names.

Since the supported signature algorithms extension is only used in TLS 1.2
and DTLS 1.2 the results for earlier versions of TLS and DTLS may not be
very useful. Applications may wish to specify a different "legacy" chain
for earlier versions of TLS or DTLS.

# SEE ALSO

[SSL\_CTX\_set\_cert\_cb(3)](http://man.he.net/man3/SSL_CTX_set_cert_cb),
[ssl(3)](http://man.he.net/man3/ssl)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
