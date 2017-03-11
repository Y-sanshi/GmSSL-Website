# NAME

CMS\_get0\_type, CMS\_set1\_eContentType, CMS\_get0\_eContentType, CMS\_get0\_content - get and set CMS content types and content

# SYNOPSIS

    #include <openssl/cms.h>

    const ASN1_OBJECT *CMS_get0_type(const CMS_ContentInfo *cms);
    int CMS_set1_eContentType(CMS_ContentInfo *cms, const ASN1_OBJECT *oid);
    const ASN1_OBJECT *CMS_get0_eContentType(CMS_ContentInfo *cms);
    ASN1_OCTET_STRING **CMS_get0_content(CMS_ContentInfo *cms);

# DESCRIPTION

CMS\_get0\_type() returns the content type of a CMS\_ContentInfo structure as
and ASN1\_OBJECT pointer. An application can then decide how to process the
CMS\_ContentInfo structure based on this value.

CMS\_set1\_eContentType() sets the embedded content type of a CMS\_ContentInfo
structure. It should be called with CMS functions with the **CMS\_PARTIAL**
flag and **before** the structure is finalised, otherwise the results are
undefined.

ASN1\_OBJECT \*CMS\_get0\_eContentType() returns a pointer to the embedded
content type.

CMS\_get0\_content() returns a pointer to the **ASN1\_OCTET\_STRING** pointer
containing the embedded content.

# NOTES

As the **0** implies CMS\_get0\_type(), CMS\_get0\_eContentType() and
CMS\_get0\_content() return internal pointers which should **not** be freed up.
CMS\_set1\_eContentType() copies the supplied OID and it **should** be freed up
after use.

The **ASN1\_OBJECT** values returned can be converted to an integer **NID** value
using OBJ\_obj2nid(). For the currently supported content types the following
values are returned:

    NID_pkcs7_data
    NID_pkcs7_signed
    NID_pkcs7_digest
    NID_id_smime_ct_compressedData:
    NID_pkcs7_encrypted
    NID_pkcs7_enveloped

The return value of CMS\_get0\_content() is a pointer to the **ASN1\_OCTET\_STRING**
content pointer. That means that for example:

    ASN1_OCTET_STRING **pconf = CMS_get0_content(cms);

**\*pconf** could be NULL if there is no embedded content. Applications can
access, modify or create the embedded content in a **CMS\_ContentInfo** structure
using this function. Applications usually will not need to modify the
embedded content as it is normally set by higher level functions.

# RETURN VALUES

CMS\_get0\_type() and CMS\_get0\_eContentType() return and ASN1\_OBJECT structure.

CMS\_set1\_eContentType() returns 1 for success or 0 if an error occurred.  The
error can be obtained from ERR\_get\_error(3).

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error)

# COPYRIGHT

Copyright 2008-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
