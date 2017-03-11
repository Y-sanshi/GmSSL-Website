# NAME

OCSP\_response\_status, OCSP\_response\_get1\_basic, OCSP\_response\_create,
OCSP\_RESPONSE\_free, OCSP\_RESPID\_set\_by\_name,
OCSP\_RESPID\_set\_by\_key, OCSP\_RESPID\_match - OCSP response functions

# SYNOPSIS

    #include <openssl/ocsp.h>

    int OCSP_response_status(OCSP_RESPONSE *resp);
    OCSP_BASICRESP *OCSP_response_get1_basic(OCSP_RESPONSE *resp);
    OCSP_RESPONSE *OCSP_response_create(int status, OCSP_BASICRESP *bs);
    void OCSP_RESPONSE_free(OCSP_RESPONSE *resp);

    int OCSP_RESPID_set_by_name(OCSP_RESPID *respid, X509 *cert);
    int OCSP_RESPID_set_by_key(OCSP_RESPID *respid, X509 *cert);
    int OCSP_RESPID_match(OCSP_RESPID *respid, X509 *cert);

# DESCRIPTION

OCSP\_response\_status() returns the OCSP response status of **resp**. It returns
one of the values: **OCSP\_RESPONSE\_STATUS\_SUCCESSFUL**,
**OCSP\_RESPONSE\_STATUS\_MALFORMEDREQUEST**,
**OCSP\_RESPONSE\_STATUS\_INTERNALERROR**, **OCSP\_RESPONSE\_STATUS\_TRYLATER**
**OCSP\_RESPONSE\_STATUS\_SIGREQUIRED**, or **OCSP\_RESPONSE\_STATUS\_UNAUTHORIZED**.

OCSP\_response\_get1\_basic() decodes and returns the **OCSP\_BASICRESP** structure
contained in **resp**.

OCSP\_response\_create() creates and returns an **OCSP\_RESPONSE** structure for
**status** and optionally including basic response **bs**.

OCSP\_RESPONSE\_free() frees up OCSP response **resp**.

OCSP\_RESPID\_set\_by\_name() sets the name of the OCSP\_RESPID to be the same as the
subject name in the supplied X509 certificate **cert** for the OCSP responder.

OCSP\_RESPID\_set\_by\_key() sets the key of the OCSP\_RESPID to be the same as the
key in the supplied X509 certificate **cert** for the OCSP responder. The key is
stored as a SHA1 hash.

Note that an OCSP\_RESPID can only have one of the name, or the key set. Calling
OCSP\_RESPID\_set\_by\_name() or OCSP\_RESPID\_set\_by\_key() will clear any existing
setting.

OCSP\_RESPID\_match() tests whether the OCSP\_RESPID given in **respid** matches
with the X509 certificate **cert**.

# RETURN VALUES

OCSP\_RESPONSE\_status() returns a status value.

OCSP\_response\_get1\_basic() returns an **OCSP\_BASICRESP** structure pointer or
**NULL** if an error occurred.

OCSP\_response\_create() returns an **OCSP\_RESPONSE** structure pointer or **NULL**
if an error occurred.

OCSP\_RESPONSE\_free() does not return a value.

OCSP\_RESPID\_set\_by\_name() and OCSP\_RESPID\_set\_by\_key() return 1 on success or 0
on failure.

OCSP\_RESPID\_match() returns 1 if the OCSP\_RESPID and the X509 certificate match
or 0 otherwise.

# NOTES

OCSP\_response\_get1\_basic() is only called if the status of a response is
**OCSP\_RESPONSE\_STATUS\_SUCCESSFUL**.

# SEE ALSO

[crypto(3)](http://man.he.net/man3/crypto)
[OCSP\_cert\_to\_id(3)](http://man.he.net/man3/OCSP_cert_to_id)
[OCSP\_request\_add1\_nonce(3)](http://man.he.net/man3/OCSP_request_add1_nonce)
[OCSP\_REQUEST\_new(3)](http://man.he.net/man3/OCSP_REQUEST_new)
[OCSP\_response\_find\_status(3)](http://man.he.net/man3/OCSP_response_find_status)
[OCSP\_sendreq\_new(3)](http://man.he.net/man3/OCSP_sendreq_new)
[OCSP\_RESPID\_new(3)](http://man.he.net/man3/OCSP_RESPID_new)
[OCSP\_RESPID\_free(3)](http://man.he.net/man3/OCSP_RESPID_free)

# HISTORY

The OCSP\_RESPID\_set\_by\_name(), OCSP\_RESPID\_set\_by\_key() and OCSP\_RESPID\_match()
functions were added in OpenSSL version 1.1.0a.

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
