# NAME

PKCS7\_sign\_add\_signer - add a signer PKCS7 signed data structure

# SYNOPSIS

    #include <openssl/pkcs7.h>

    PKCS7_SIGNER_INFO *PKCS7_sign_add_signer(PKCS7 *p7, X509 *signcert, EVP_PKEY *pkey, const EVP_MD *md, int flags);

# DESCRIPTION

PKCS7\_sign\_add\_signer() adds a signer with certificate **signcert** and private
key **pkey** using message digest **md** to a PKCS7 signed data structure
**p7**.

The PKCS7 structure should be obtained from an initial call to PKCS7\_sign()
with the flag **PKCS7\_PARTIAL** set or in the case or re-signing a valid PKCS7
signed data structure.

If the **md** parameter is **NULL** then the default digest for the public
key algorithm will be used.

Unless the **PKCS7\_REUSE\_DIGEST** flag is set the returned PKCS7 structure
is not complete and must be finalized either by streaming (if applicable) or
a call to PKCS7\_final().

# NOTES

The main purpose of this function is to provide finer control over a PKCS#7
signed data structure where the simpler PKCS7\_sign() function defaults are
not appropriate. For example if multiple signers or non default digest
algorithms are needed.

Any of the following flags (ored together) can be passed in the **flags**
parameter.

If **PKCS7\_REUSE\_DIGEST** is set then an attempt is made to copy the content
digest value from the PKCS7 structure: to add a signer to an existing structure.
An error occurs if a matching digest value cannot be found to copy. The
returned PKCS7 structure will be valid and finalized when this flag is set.

If **PKCS7\_PARTIAL** is set in addition to **PKCS7\_REUSE\_DIGEST** then the
**PKCS7\_SIGNER\_INO** structure will not be finalized so additional attributes
can be added. In this case an explicit call to PKCS7\_SIGNER\_INFO\_sign() is
needed to finalize it.

If **PKCS7\_NOCERTS** is set the signer's certificate will not be included in the
PKCS7 structure, the signer's certificate must still be supplied in the
**signcert** parameter though. This can reduce the size of the signature if the
signers certificate can be obtained by other means: for example a previously
signed message.

The signedData structure includes several PKCS#7 autenticatedAttributes
including the signing time, the PKCS#7 content type and the supported list of
ciphers in an SMIMECapabilities attribute. If **PKCS7\_NOATTR** is set then no
authenticatedAttributes will be used. If **PKCS7\_NOSMIMECAP** is set then just
the SMIMECapabilities are omitted.

If present the SMIMECapabilities attribute indicates support for the following
algorithms: triple DES, 128 bit RC2, 64 bit RC2, DES and 40 bit RC2. If any of
these algorithms is disabled then it will not be included.

PKCS7\_sign\_add\_signers() returns an internal pointer to the PKCS7\_SIGNER\_INFO
structure just added, this can be used to set additional attributes
before it is finalized.

# RETURN VALUES

PKCS7\_sign\_add\_signers() returns an internal pointer to the PKCS7\_SIGNER\_INFO
structure just added or NULL if an error occurs.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [PKCS7\_sign(3)](http://man.he.net/man3/PKCS7_sign),
[PKCS7\_final(3)](http://man.he.net/man3/PKCS7_final),

# HISTORY

PPKCS7\_sign\_add\_signer() was added to OpenSSL 1.0.0

# COPYRIGHT

Copyright 2007-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
