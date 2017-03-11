# NAME

SCT\_validate, SCT\_LIST\_validate, SCT\_get\_validation\_status -
checks Signed Certificate Timestamps (SCTs) are valid

# SYNOPSIS

    #include <openssl/ct.h>

    typedef enum {
     SCT_VALIDATION_STATUS_NOT_SET,
     SCT_VALIDATION_STATUS_UNKNOWN_LOG,
     SCT_VALIDATION_STATUS_VALID,
     SCT_VALIDATION_STATUS_INVALID,
     SCT_VALIDATION_STATUS_UNVERIFIED,
     SCT_VALIDATION_STATUS_UNKNOWN_VERSION
    } sct_validation_status_t;

    int SCT_validate(SCT *sct, const CT_POLICY_EVAL_CTX *ctx);
    int SCT_LIST_validate(const STACK_OF(SCT) *scts, CT_POLICY_EVAL_CTX *ctx);
    sct_validation_status_t SCT_get_validation_status(const SCT *sct);

# DESCRIPTION

SCT\_validate() will check that an SCT is valid and verify its signature.
SCT\_LIST\_validate() performs the same checks on an entire stack of SCTs.
The result of the validation checks can be obtained by passing the SCT to
SCT\_get\_validation\_status().

A CT\_POLICY\_EVAL\_CTX must be provided that specifies:

- The certificate the SCT was issued for.

    Failure to provide the certificate will result in the validation status being
    SCT\_VALIDATION\_STATUS\_UNVERIFIED.

- The issuer of that certificate.

    This is only required if the SCT was issued for a pre-certificate
    (see RFC 6962). If it is required but not provided, the validation status will
    be SCT\_VALIDATION\_STATUS\_UNVERIFIED.

- A CTLOG\_STORE that contains the CT log that issued this SCT.

    If the SCT was issued by a log that is not in this CTLOG\_STORE, the validation
    status will be SCT\_VALIDATION\_STATUS\_UNKNOWN\_LOG.

If the SCT is of an unsupported version (only v1 is currently supported), the
validation status will be SCT\_VALIDATION\_STATUS\_UNKNOWN\_VERSION.

If the SCT's signature is incorrect, the validation status will be
SCT\_VALIDATION\_STATUS\_INVALID. Otherwise, if all checks have passed, the
validation status will be SCT\_VALIDATION\_STATUS\_VALID.

# NOTES

A return value of 0 from SCT\_LIST\_validate() should not be interpreted as a
failure. At a minimum, only one valid SCT may provide sufficient confidence
that a certificate has been publicly logged.

# RETURN VALUES

SCT\_validate() returns a negative integer if an internal error occurs, 0 if the
SCT fails validation, or 1 if the SCT passes validation.

SCT\_LIST\_validate() returns a negative integer if an internal error occurs, 0
if any of SCTs fails validation, or 1 if they all pass validation.

SCT\_get\_validation\_status() returns the validation status of the SCT.
If SCT\_validate() or SCT\_LIST\_validate() have not been passed that SCT, the
returned value will be SCT\_VALIDATION\_STATUS\_NOT\_SET.

# SEE ALSO

[ct(3)](http://man.he.net/man3/ct)

# HISTORY

These functions were added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
