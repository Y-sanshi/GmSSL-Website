# NAME

SSL\_get\_shared\_sigalgs, SSL\_get\_sigalgs - get supported signature algorithms

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_get_shared_sigalgs(SSL *s, int idx,
                               int *psign, int *phash, int *psignhash,
                               unsigned char *rsig, unsigned char *rhash);

    int SSL_get_sigalgs(SSL *s, int idx,
                        int *psign, int *phash, int *psignhash,
                        unsigned char *rsig, unsigned char *rhash);

# DESCRIPTION

SSL\_get\_shared\_sigalgs() returns information about the shared signature
algorithms supported by peer **s**. The parameter **idx** indicates the index
of the shared signature algorithm to return starting from zero. The signature
algorithm NID is written to **\*psign**, the hash NID to **\*phash** and the
sign and hash NID to **\*psignhash**. The raw signature and hash values
are written to **\*rsig** and **\*rhash**.

SSL\_get\_sigalgs() is similar to SSL\_get\_shared\_sigalgs() except it returns
information about all signature algorithms supported by **s** in the order
they were sent by the peer.

# RETURN VALUES

SSL\_get\_shared\_sigalgs() and SSL\_get\_sigalgs() return the number of
signature algorithms or **0** if the **idx** parameter is out of range.

# NOTES

These functions are typically called for debugging purposes (to report
the peer's preferences) or where an application wants finer control over
certificate selection. Most applications will rely on internal handling
and will not need to call them.

If an application is only interested in the highest preference shared
signature algorithm it can just set **idx** to zero.

Any or all of the parameters **psign**, **phash**, **psignhash**, **rsig** or
**rhash** can be set to **NULL** if the value is not required. By setting
them all to **NULL** and setting **idx** to zero the total number of
signature algorithms can be determined: which can be zero.

These functions must be called after the peer has sent a list of supported
signature algorithms: after a client hello (for servers) or a certificate
request (for clients). They can (for example) be called in the certificate
callback.

Only TLS 1.2 and DTLS 1.2 currently support signature algorithms. If these
functions are called on an earlier version of TLS or DTLS zero is returned.

The shared signature algorithms returned by SSL\_get\_shared\_sigalgs() are
ordered according to configuration and peer preferences.

The raw values correspond to the on the wire form as defined by RFC5246 et al.
The NIDs are OpenSSL equivalents. For example if the peer sent sha256(4) and
rsa(1) then **\*rhash** would be 4, **\*rsign** 1, **\*phash** NID\_sha256, **\*psig**
NID\_rsaEncryption and **\*psighash** NID\_sha256WithRSAEncryption.

If a signature algorithm is not recognised the corresponding NIDs
will be set to **NID\_undef**. This may be because the value is not supported
or is not an appropriate combination (for example MD5 and DSA).

# SEE ALSO

[SSL\_CTX\_set\_cert\_cb(3)](http://man.he.net/man3/SSL_CTX_set_cert_cb),
[ssl(3)](http://man.he.net/man3/ssl)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
