# NAME

OCSP\_resp\_get0\_certs,
OCSP\_resp\_get0\_id,
OCSP\_resp\_get0\_produced\_at,
OCSP\_resp\_find\_status, OCSP\_resp\_count, OCSP\_resp\_get0, OCSP\_resp\_find,
OCSP\_single\_get0\_status, OCSP\_check\_validity
\- OCSP response utility functions

# SYNOPSIS

    #include <openssl/ocsp.h>

    int OCSP_resp_find_status(OCSP_BASICRESP *bs, OCSP_CERTID *id, int *status,
                              int *reason,
                              ASN1_GENERALIZEDTIME **revtime,
                              ASN1_GENERALIZEDTIME **thisupd,
                              ASN1_GENERALIZEDTIME **nextupd);

    int OCSP_resp_count(OCSP_BASICRESP *bs);
    OCSP_SINGLERESP *OCSP_resp_get0(OCSP_BASICRESP *bs, int idx);
    int OCSP_resp_find(OCSP_BASICRESP *bs, OCSP_CERTID *id, int last);
    int OCSP_single_get0_status(OCSP_SINGLERESP *single, int *reason,
                                ASN1_GENERALIZEDTIME **revtime,
                                ASN1_GENERALIZEDTIME **thisupd,
                                ASN1_GENERALIZEDTIME **nextupd);

    const ASN1_GENERALIZEDTIME *OCSP_resp_get0_produced_at(
                                const OCSP_BASICRESP* single);

    const STACK_OF(X509) *OCSP_resp_get0_certs(const OCSP_BASICRESP *bs);

    int OCSP_resp_get0_id(const OCSP_BASICRESP *bs,
                          const ASN1_OCTET_STRING **pid,
                          const X509_NAME **pname);

    int OCSP_check_validity(ASN1_GENERALIZEDTIME *thisupd,
                            ASN1_GENERALIZEDTIME *nextupd,
                            long sec, long maxsec);

# DESCRIPTION

OCSP\_resp\_find\_status() searches **bs** for an OCSP response for **id**. If it is
successful the fields of the response are returned in **\*status**, **\*reason**,
**\*revtime**, **\*thisupd** and **\*nextupd**.  The **\*status** value will be one of
**V\_OCSP\_CERTSTATUS\_GOOD**, **V\_OCSP\_CERTSTATUS\_REVOKED** or
**V\_OCSP\_CERTSTATUS\_UNKNOWN**. The **\*reason** and **\*revtime** fields are only
set if the status is **V\_OCSP\_CERTSTATUS\_REVOKED**. If set the **\*reason** field
will be set to the revocation reason which will be one of
**OCSP\_REVOKED\_STATUS\_NOSTATUS**, **OCSP\_REVOKED\_STATUS\_UNSPECIFIED**,
**OCSP\_REVOKED\_STATUS\_KEYCOMPROMISE**, **OCSP\_REVOKED\_STATUS\_CACOMPROMISE**,
**OCSP\_REVOKED\_STATUS\_AFFILIATIONCHANGED**, **OCSP\_REVOKED\_STATUS\_SUPERSEDED**,
**OCSP\_REVOKED\_STATUS\_CESSATIONOFOPERATION**,
**OCSP\_REVOKED\_STATUS\_CERTIFICATEHOLD** or **OCSP\_REVOKED\_STATUS\_REMOVEFROMCRL**.

OCSP\_resp\_count() returns the number of **OCSP\_SINGLERESP** structures in **bs**.

OCSP\_resp\_get0() returns the **OCSP\_SINGLERESP** structure in **bs**
corresponding to index **idx**. Where **idx** runs from 0 to
OCSP\_resp\_count(bs) - 1.

OCSP\_resp\_find() searches **bs** for **id** and returns the index of the first
matching entry after **last** or starting from the beginning if **last** is -1.

OCSP\_single\_get0\_status() extracts the fields of **single** in **\*reason**,
**\*revtime**, **\*thisupd** and **\*nextupd**.

OCSP\_resp\_get0\_produced\_at() extracts the **producedAt** field from the
single response **bs**.

OCSP\_resp\_get0\_certs() returns any certificates included in **bs**.

OCSP\_resp\_get0\_id() gets the responder id of &lt;bs>. If the responder ID is
a name then <\*pname> is set to the name and **\*pid** is set to NULL. If the
responder ID is by key ID then **\*pid** is set to the key ID and **\*pname**
is set to NULL.

OCSP\_check\_validity() checks the validity of **thisupd** and **nextupd** values
which will be typically obtained from OCSP\_resp\_find\_status() or
OCSP\_single\_get0\_status(). If **sec** is non-zero it indicates how many seconds
leeway should be allowed in the check. If **maxsec** is positive it indicates
the maximum age of **thisupd** in seconds.

# RETURN VALUES

OCSP\_resp\_find\_status() returns 1 if **id** is found in **bs** and 0 otherwise.

OCSP\_resp\_count() returns the total number of **OCSP\_SINGLERESP** fields in
**bs**.

OCSP\_resp\_get0() returns a pointer to an **OCSP\_SINGLERESP** structure or
**NULL** if **idx** is out of range.

OCSP\_resp\_find() returns the index of **id** in **bs** (which may be 0) or -1 if
**id** was not found.

OCSP\_single\_get0\_status() returns the status of **single** or -1 if an error
occurred.

# NOTES

Applications will typically call OCSP\_resp\_find\_status() using the certificate
ID of interest and then check its validity using OCSP\_check\_validity(). They
can then take appropriate action based on the status of the certificate.

An OCSP response for a certificate contains **thisUpdate** and **nextUpdate**
fields. Normally the current time should be between these two values. To
account for clock skew the **maxsec** field can be set to non-zero in
OCSP\_check\_validity(). Some responders do not set the **nextUpdate** field, this
would otherwise mean an ancient response would be considered valid: the
**maxsec** parameter to OCSP\_check\_validity() can be used to limit the permitted
age of responses.

The values written to **\*revtime**, **\*thisupd** and **\*nextupd** by
OCSP\_resp\_find\_status() and OCSP\_single\_get0\_status() are internal pointers
which **MUST NOT** be freed up by the calling application. Any or all of these
parameters can be set to NULL if their value is not required.

# SEE ALSO

[crypto(3)](http://man.he.net/man3/crypto),
[OCSP\_cert\_to\_id(3)](http://man.he.net/man3/OCSP_cert_to_id),
[OCSP\_request\_add1\_nonce(3)](http://man.he.net/man3/OCSP_request_add1_nonce),
[OCSP\_REQUEST\_new(3)](http://man.he.net/man3/OCSP_REQUEST_new),
[OCSP\_response\_status(3)](http://man.he.net/man3/OCSP_response_status),
[OCSP\_sendreq\_new(3)](http://man.he.net/man3/OCSP_sendreq_new)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
