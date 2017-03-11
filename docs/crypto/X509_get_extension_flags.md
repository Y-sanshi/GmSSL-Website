# NAME

X509\_get0\_subject\_key\_id,
X509\_get\_pathlen,
X509\_get\_extension\_flags,
X509\_get\_key\_usage,
X509\_get\_extended\_key\_usage,
X509\_set\_proxy\_flag,
X509\_set\_proxy\_pathlen,
X509\_get\_proxy\_pathlen - retrieve certificate extension data

# SYNOPSIS

    #include <openssl/x509v3.h>

    long X509_get_pathlen(X509 *x);
    uint32_t X509_get_extension_flags(X509 *x);
    uint32_t X509_get_key_usage(X509 *x);
    uint32_t X509_get_extended_key_usage(X509 *x);
    const ASN1_OCTET_STRING *X509_get0_subject_key_id(X509 *x);
    void X509_set_proxy_flag(X509 *x);
    void X509_set_proxy_pathlen(int l);
    long X509_get_proxy_pathlen(X509 *x);

# DESCRIPTION

These functions retrieve information related to commonly used certificate extensions.

X509\_get\_pathlen() retrieves the path length extension from a certificate.
This extension is used to limit the length of a cert chain that may be
issued from that CA.

X509\_get\_extension\_flags() retrieves general information about a certificate,
it will return one or more of the following flags ored together.

- **EXFLAG\_V1**

    The certificate is an obsolete version 1 certificate.

- **EXFLAG\_BCONS**

    The certificate contains a basic constraints extension.

- **EXFLAG\_CA**

    The certificate contains basic constraints and asserts the CA flag.

- **EXFLAG\_PROXY**

    The certificate is a valid proxy certificate.

- **EXFLAG\_SI**

    The certificate is self issued (that is subject and issuer names match).

- **EXFLAG\_SS**

    The subject and issuer names match and extension values imply it is self
    signed.

- **EXFLAG\_FRESHEST**

    The freshest CRL extension is present in the certificate.

- **EXFLAG\_CRITICAL**

    The certificate contains an unhandled critical extension.

- **EXFLAG\_INVALID**

    Some certificate extension values are invalid or inconsistent. The
    certificate should be rejected.

- **EXFLAG\_KUSAGE**

    The certificate contains a key usage extension. The value can be retrieved
    using X509\_get\_key\_usage().

- **EXFLAG\_XKUSAGE**

    The certificate contains an extended key usage extension. The value can be
    retrieved using X509\_get\_extended\_key\_usage().

X509\_get\_key\_usage() returns the value of the key usage extension.  If key
usage is present will return zero or more of the flags:
**KU\_DIGITAL\_SIGNATURE**, **KU\_NON\_REPUDIATION**, **KU\_KEY\_ENCIPHERMENT**,
**KU\_DATA\_ENCIPHERMENT**, **KU\_KEY\_AGREEMENT**, **KU\_KEY\_CERT\_SIGN**,
**KU\_CRL\_SIGN**, **KU\_ENCIPHER\_ONLY** or **KU\_DECIPHER\_ONLY** corresponding to
individual key usage bits. If key usage is absent then **UINT32\_MAX** is
returned.

X509\_get\_extended\_key\_usage() returns the value of the extended key usage
extension. If extended key usage is present it will return zero or more of the
flags: **XKU\_SSL\_SERVER**, **XKU\_SSL\_CLIENT**, **XKU\_SMIME**, **XKU\_CODE\_SIGN**
**XKU\_OCSP\_SIGN**, **XKU\_TIMESTAMP**, **XKU\_DVCS** or **XKU\_ANYEKU**. These
correspond to the OIDs **id-kp-serverAuth**, **id-kp-clientAuth**,
**id-kp-emailProtection**, **id-kp-codeSigning**, **id-kp-OCSPSigning**,
**id-kp-timeStamping**, **id-kp-dvcs** and **anyExtendedKeyUsage** respectively.
Additionally **XKU\_SGC** is set if either Netscape or Microsoft SGC OIDs are
present.

X509\_get\_extended\_key\_usage() return an internal pointer to the subject key
identifier of **x** as an **ASN1\_OCTET\_STRING** or **NULL** if the extension
is not present or cannot be parsed.

X509\_set\_proxy\_flag() marks the certificate with the **EXFLAG\_PROXY** flag.
This is for the users who need to mark non-RFC3820 proxy certificates as
such, as OpenSSL only detects RFC3820 compliant ones.

X509\_set\_proxy\_pathlen() sets the proxy certificate path length for the given
certificate **x**.  This is for the users who need to mark non-RFC3820 proxy
certificates as such, as OpenSSL only detects RFC3820 compliant ones.

X509\_get\_proxy\_pathlen() returns the proxy certificate path length for the
given certificate **x** if it is a proxy certificate.

# NOTES

The value of the flags correspond to extension values which are cached
in the **X509** structure. If the flags returned do not provide sufficient
information an application should examine extension values directly
for example using X509\_get\_ext\_d2i().

If the key usage or extended key usage extension is absent then typically usage
is unrestricted. For this reason X509\_get\_key\_usage() and
X509\_get\_extended\_key\_usage() return **UINT32\_MAX** when the corresponding
extension is absent. Applications can additionally check the return value of
X509\_get\_extension\_flags() and take appropriate action is an extension is
absent.

If X509\_get0\_subject\_key\_id() returns **NULL** then the extension may be
absent or malformed. Applications can determine the precise reason using
X509\_get\_ext\_d2i().

# RETURN VALUE

X509\_get\_pathlen() returns the path length value, or -1 if the extension
is not present.

X509\_get\_extension\_flags(), X509\_get\_key\_usage() and
X509\_get\_extended\_key\_usage() return sets of flags corresponding to the
certificate extension values.

X509\_get0\_subject\_key\_id() returns the subject key identifier as a
pointer to an **ASN1\_OCTET\_STRING** structure or **NULL** if the extension
is absent or an error occurred during parsing.

X509\_get\_proxy\_pathlen() returns the path length value if the given
certificate is a proxy one and has a path length set, and -1 otherwise.

# SEE ALSO

[X509\_check\_purpose(3)](http://man.he.net/man3/X509_check_purpose)

# HISTORY

X509\_get\_pathlen(), X509\_set\_proxy\_flag(), X509\_set\_proxy\_pathlen() and
X509\_get\_proxy\_pathlen() were added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
