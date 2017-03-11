# NAME

OCSP\_cert\_to\_id, OCSP\_cert\_id\_new, OCSP\_CERTID\_free, OCSP\_id\_issuer\_cmp,
OCSP\_id\_cmp, OCSP\_id\_get0\_info - OCSP certificate ID utility functions

# SYNOPSIS

    #include <openssl/ocsp.h>

    OCSP_CERTID *OCSP_cert_to_id(const EVP_MD *dgst,
                                 X509 *subject, X509 *issuer);

    OCSP_CERTID *OCSP_cert_id_new(const EVP_MD *dgst,
                                  X509_NAME *issuerName,
                                  ASN1_BIT_STRING *issuerKey,
                                  ASN1_INTEGER *serialNumber);

    void OCSP_CERTID_free(OCSP_CERTID *id);

    int OCSP_id_issuer_cmp(OCSP_CERTID *a, OCSP_CERTID *b);
    int OCSP_id_cmp(OCSP_CERTID *a, OCSP_CERTID *b);

    int OCSP_id_get0_info(ASN1_OCTET_STRING **piNameHash, ASN1_OBJECT **pmd,
                          ASN1_OCTET_STRING **pikeyHash,
                          ASN1_INTEGER **pserial, OCSP_CERTID *cid);

# DESCRIPTION

OCSP\_cert\_to\_id() creates and returns a new **OCSP\_CERTID** structure using
message digest **dgst** for certificate **subject** with issuer **issuer**. If
**dgst** is **NULL** then SHA1 is used.

OCSP\_cert\_id\_new() creates and returns a new **OCSP\_CERTID** using **dgst** and
issuer name **issuerName**, issuer key hash **issuerKey** and serial number
**serialNumber**.

OCSP\_CERTID\_free() frees up **id**.

OCSP\_id\_cmp() compares **OCSP\_CERTID** **a** and **b**.

OCSP\_id\_issuer\_cmp() compares only the issuer name of **OCSP\_CERTID** **a** and **b**.

OCSP\_id\_get0\_info() returns the issuer name hash, hash OID, issuer key hash and
serial number contained in **cid**. If any of the values are not required the
corresponding parameter can be set to **NULL**.

# RETURN VALUES

OCSP\_cert\_to\_id() and OCSP\_cert\_id\_new() return either a pointer to a valid
**OCSP\_CERTID** structure or **NULL** if an error occurred.

OCSP\_id\_cmp() and OCSP\_id\_issuer\_cmp() returns zero for a match and non-zero
otherwise.

OCSP\_CERTID\_free() does not return a value.

OCSP\_id\_get0\_info() returns 1 for success and 0 for failure.

# NOTES

OCSP clients will typically only use OCSP\_cert\_to\_id() or OCSP\_cert\_id\_new():
the other functions are used by responder applications.

The values returned by OCSP\_id\_get0\_info() are internal pointers and **MUST
NOT** be freed up by an application: they will be freed when the corresponding
**OCSP\_CERTID** structure is freed.

# SEE ALSO

[crypto(3)](http://man.he.net/man3/crypto),
[OCSP\_request\_add1\_nonce(3)](http://man.he.net/man3/OCSP_request_add1_nonce),
[OCSP\_REQUEST\_new(3)](http://man.he.net/man3/OCSP_REQUEST_new),
[OCSP\_response\_find\_status(3)](http://man.he.net/man3/OCSP_response_find_status),
[OCSP\_response\_status(3)](http://man.he.net/man3/OCSP_response_status),
[OCSP\_sendreq\_new(3)](http://man.he.net/man3/OCSP_sendreq_new)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
