# NAME

CMS\_verify\_receipt - verify a CMS signed receipt

# SYNOPSIS

    #include <openssl/cms.h>

    int CMS_verify_receipt(CMS_ContentInfo *rcms, CMS_ContentInfo *ocms, STACK_OF(X509) *certs, X509_STORE *store, unsigned int flags);

# DESCRIPTION

CMS\_verify\_receipt() verifies a CMS signed receipt. **rcms** is the signed
receipt to verify. **ocms** is the original SignedData structure containing the
receipt request. **certs** is a set of certificates in which to search for the
signing certificate. **store** is a trusted certificate store (used for chain
verification).

**flags** is an optional set of flags, which can be used to modify the verify
operation.

# NOTES

This functions behaves in a similar way to CMS\_verify() except the flag values
**CMS\_DETACHED**, **CMS\_BINARY**, **CMS\_TEXT** and **CMS\_STREAM** are not
supported since they do not make sense in the context of signed receipts.

# RETURN VALUES

CMS\_verify\_receipt() returns 1 for a successful verification and zero if an
error occurred.

The error can be obtained from [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error)

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[CMS\_sign\_receipt(3)](http://man.he.net/man3/CMS_sign_receipt),
[CMS\_verify(3)](http://man.he.net/man3/CMS_verify),

# COPYRIGHT

Copyright 2008-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
