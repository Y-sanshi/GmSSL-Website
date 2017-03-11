# NAME

CMS\_add1\_signer, CMS\_SignerInfo\_sign - add a signer to a CMS\_ContentInfo signed data structure

# SYNOPSIS

    #include <openssl/cms.h>

    CMS_SignerInfo *CMS_add1_signer(CMS_ContentInfo *cms, X509 *signcert, EVP_PKEY *pkey, const EVP_MD *md, unsigned int flags);

    int CMS_SignerInfo_sign(CMS_SignerInfo *si);

# DESCRIPTION

CMS\_add1\_signer() adds a signer with certificate **signcert** and private
key **pkey** using message digest **md** to CMS\_ContentInfo SignedData
structure **cms**.

The CMS\_ContentInfo structure should be obtained from an initial call to
CMS\_sign() with the flag **CMS\_PARTIAL** set or in the case or re-signing a
valid CMS\_ContentInfo SignedData structure.

If the **md** parameter is **NULL** then the default digest for the public
key algorithm will be used.

Unless the **CMS\_REUSE\_DIGEST** flag is set the returned CMS\_ContentInfo
structure is not complete and must be finalized either by streaming (if
applicable) or a call to CMS\_final().

The CMS\_SignerInfo\_sign() function will explicitly sign a CMS\_SignerInfo
structure, its main use is when **CMS\_REUSE\_DIGEST** and **CMS\_PARTIAL** flags
are both set.

# NOTES

The main purpose of CMS\_add1\_signer() is to provide finer control
over a CMS signed data structure where the simpler CMS\_sign() function defaults
are not appropriate. For example if multiple signers or non default digest
algorithms are needed. New attributes can also be added using the returned
CMS\_SignerInfo structure and the CMS attribute utility functions or the
CMS signed receipt request functions.

Any of the following flags (ored together) can be passed in the **flags**
parameter.

If **CMS\_REUSE\_DIGEST** is set then an attempt is made to copy the content
digest value from the CMS\_ContentInfo structure: to add a signer to an existing
structure.  An error occurs if a matching digest value cannot be found to copy.
The returned CMS\_ContentInfo structure will be valid and finalized when this
flag is set.

If **CMS\_PARTIAL** is set in addition to **CMS\_REUSE\_DIGEST** then the
CMS\_SignerInfo structure will not be finalized so additional attributes
can be added. In this case an explicit call to CMS\_SignerInfo\_sign() is
needed to finalize it.

If **CMS\_NOCERTS** is set the signer's certificate will not be included in the
CMS\_ContentInfo structure, the signer's certificate must still be supplied in
the **signcert** parameter though. This can reduce the size of the signature if
the signers certificate can be obtained by other means: for example a
previously signed message.

The SignedData structure includes several CMS signedAttributes including the
signing time, the CMS content type and the supported list of ciphers in an
SMIMECapabilities attribute. If **CMS\_NOATTR** is set then no signedAttributes
will be used. If **CMS\_NOSMIMECAP** is set then just the SMIMECapabilities are
omitted.

OpenSSL will by default identify signing certificates using issuer name
and serial number. If **CMS\_USE\_KEYID** is set it will use the subject key
identifier value instead. An error occurs if the signing certificate does not
have a subject key identifier extension.

If present the SMIMECapabilities attribute indicates support for the following
algorithms in preference order: 256 bit AES, Gost R3411-94, Gost 28147-89, 192
bit AES, 128 bit AES, triple DES, 128 bit RC2, 64 bit RC2, DES and 40 bit RC2.
If any of these algorithms is not available then it will not be included: for example the GOST algorithms will not be included if the GOST ENGINE is
not loaded.

CMS\_add1\_signer() returns an internal pointer to the CMS\_SignerInfo
structure just added, this can be used to set additional attributes
before it is finalized.

# RETURN VALUES

CMS\_add1\_signer() returns an internal pointer to the CMS\_SignerInfo
structure just added or NULL if an error occurs.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [CMS\_sign(3)](http://man.he.net/man3/CMS_sign),
[CMS\_final(3)](http://man.he.net/man3/CMS_final),

# COPYRIGHT

Copyright 2014-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
