# NAME

CMS\_add1\_recipient\_cert, CMS\_add0\_recipient\_key - add recipients to a CMS enveloped data structure

# SYNOPSIS

    #include <openssl/cms.h>

    CMS_RecipientInfo *CMS_add1_recipient_cert(CMS_ContentInfo *cms, X509 *recip, unsigned int flags);

    CMS_RecipientInfo *CMS_add0_recipient_key(CMS_ContentInfo *cms, int nid, unsigned char *key, size_t keylen, unsigned char *id, size_t idlen, ASN1_GENERALIZEDTIME *date, ASN1_OBJECT *otherTypeId, ASN1_TYPE *otherType);

# DESCRIPTION

CMS\_add1\_recipient\_cert() adds recipient **recip** to CMS\_ContentInfo enveloped
data structure **cms** as a KeyTransRecipientInfo structure.

CMS\_add0\_recipient\_key() adds symmetric key **key** of length **keylen** using
wrapping algorithm **nid**, identifier **id** of length **idlen** and optional
values **date**, **otherTypeId** and **otherType** to CMS\_ContentInfo enveloped
data structure **cms** as a KEKRecipientInfo structure.

The CMS\_ContentInfo structure should be obtained from an initial call to
CMS\_encrypt() with the flag **CMS\_PARTIAL** set.

# NOTES

The main purpose of this function is to provide finer control over a CMS
enveloped data structure where the simpler CMS\_encrypt() function defaults are
not appropriate. For example if one or more KEKRecipientInfo structures
need to be added. New attributes can also be added using the returned
CMS\_RecipientInfo structure and the CMS attribute utility functions.

OpenSSL will by default identify recipient certificates using issuer name
and serial number. If **CMS\_USE\_KEYID** is set it will use the subject key
identifier value instead. An error occurs if all recipient certificates do not
have a subject key identifier extension.

Currently only AES based key wrapping algorithms are supported for **nid**,
specifically: NID\_id\_aes128\_wrap, NID\_id\_aes192\_wrap and NID\_id\_aes256\_wrap.
If **nid** is set to **NID\_undef** then an AES wrap algorithm will be used
consistent with **keylen**.

# RETURN VALUES

CMS\_add1\_recipient\_cert() and CMS\_add0\_recipient\_key() return an internal
pointer to the CMS\_RecipientInfo structure just added or NULL if an error
occurs.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [CMS\_decrypt(3)](http://man.he.net/man3/CMS_decrypt),
[CMS\_final(3)](http://man.he.net/man3/CMS_final),

# COPYRIGHT

Copyright 2008-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
