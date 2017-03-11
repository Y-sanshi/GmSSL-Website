# NAME

CMS\_ReceiptRequest\_create0, CMS\_add1\_ReceiptRequest, CMS\_get1\_ReceiptRequest, CMS\_ReceiptRequest\_get0\_values - CMS signed receipt request functions

# SYNOPSIS

    #include <openssl/cms.h>

    CMS_ReceiptRequest *CMS_ReceiptRequest_create0(unsigned char *id, int idlen, int allorfirst, STACK_OF(GENERAL_NAMES) *receiptList, STACK_OF(GENERAL_NAMES) *receiptsTo);
    int CMS_add1_ReceiptRequest(CMS_SignerInfo *si, CMS_ReceiptRequest *rr);
    int CMS_get1_ReceiptRequest(CMS_SignerInfo *si, CMS_ReceiptRequest **prr);
    void CMS_ReceiptRequest_get0_values(CMS_ReceiptRequest *rr, ASN1_STRING **pcid, int *pallorfirst, STACK_OF(GENERAL_NAMES) **plist, STACK_OF(GENERAL_NAMES) **prto);

# DESCRIPTION

CMS\_ReceiptRequest\_create0() creates a signed receipt request structure. The
**signedContentIdentifier** field is set using **id** and **idlen**, or it is set
to 32 bytes of pseudo random data if **id** is NULL. If **receiptList** is NULL
the allOrFirstTier option in **receiptsFrom** is used and set to the value of
the **allorfirst** parameter. If **receiptList** is not NULL the **receiptList**
option in **receiptsFrom** is used. The **receiptsTo** parameter specifies the
**receiptsTo** field value.

The CMS\_add1\_ReceiptRequest() function adds a signed receipt request **rr**
to SignerInfo structure **si**.

int CMS\_get1\_ReceiptRequest() looks for a signed receipt request in **si**, if
any is found it is decoded and written to **prr**.

CMS\_ReceiptRequest\_get0\_values() retrieves the values of a receipt request.
The signedContentIdentifier is copied to **pcid**. If the **allOrFirstTier**
option of **receiptsFrom** is used its value is copied to **pallorfirst**
otherwise the **receiptList** field is copied to **plist**. The **receiptsTo**
parameter is copied to **prto**.

# NOTES

For more details of the meaning of the fields see RFC2634.

The contents of a signed receipt should only be considered meaningful if the
corresponding CMS\_ContentInfo structure can be successfully verified using
CMS\_verify().

# RETURN VALUES

CMS\_ReceiptRequest\_create0() returns a signed receipt request structure or
NULL if an error occurred.

CMS\_add1\_ReceiptRequest() returns 1 for success or 0 is an error occurred.

CMS\_get1\_ReceiptRequest() returns 1 is a signed receipt request is found and
decoded. It returns 0 if a signed receipt request is not present and -1 if
it is present but malformed.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [CMS\_sign(3)](http://man.he.net/man3/CMS_sign),
[CMS\_sign\_receipt(3)](http://man.he.net/man3/CMS_sign_receipt), [CMS\_verify(3)](http://man.he.net/man3/CMS_verify)
[CMS\_verify\_receipt(3)](http://man.he.net/man3/CMS_verify_receipt)

# COPYRIGHT

Copyright 2008-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
