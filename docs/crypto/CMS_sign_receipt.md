# NAME

CMS\_sign\_receipt - create a CMS signed receipt

# SYNOPSIS

    #include <openssl/cms.h>

    CMS_ContentInfo *CMS_sign_receipt(CMS_SignerInfo *si, X509 *signcert, EVP_PKEY *pkey, STACK_OF(X509) *certs, unsigned int flags);

# DESCRIPTION

CMS\_sign\_receipt() creates and returns a CMS signed receipt structure. **si** is
the **CMS\_SignerInfo** structure containing the signed receipt request.
**signcert** is the certificate to sign with, **pkey** is the corresponding
private key.  **certs** is an optional additional set of certificates to include
in the CMS structure (for example any intermediate CAs in the chain).

**flags** is an optional set of flags.

# NOTES

This functions behaves in a similar way to CMS\_sign() except the flag values
**CMS\_DETACHED**, **CMS\_BINARY**, **CMS\_NOATTR**, **CMS\_TEXT** and **CMS\_STREAM**
are not supported since they do not make sense in the context of signed
receipts.

# RETURN VALUES

CMS\_sign\_receipt() returns either a valid CMS\_ContentInfo structure or NULL if
an error occurred.  The error can be obtained from ERR\_get\_error(3).

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[CMS\_verify\_receipt(3)](http://man.he.net/man3/CMS_verify_receipt),
[CMS\_sign(3)](http://man.he.net/man3/CMS_sign)

# COPYRIGHT

Copyright 2008-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
