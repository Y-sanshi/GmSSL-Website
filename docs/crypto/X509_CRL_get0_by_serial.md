# NAME

X509\_CRL\_get0\_by\_serial, X509\_CRL\_get0\_by\_cert, X509\_CRL\_get\_REVOKED,
X509\_REVOKED\_get0\_serialNumber, X509\_REVOKED\_get0\_revocationDate,
X509\_REVOKED\_set\_serialNumber, X509\_REVOKED\_set\_revocationDate,
X509\_CRL\_add0\_revoked, X509\_CRL\_sort - CRL revoked entry utility
functions

# SYNOPSIS

    #include <openssl/x509.h>

    int X509_CRL_get0_by_serial(X509_CRL *crl,
                                X509_REVOKED **ret, ASN1_INTEGER *serial);
    int X509_CRL_get0_by_cert(X509_CRL *crl, X509_REVOKED **ret, X509 *x);

    STACK_OF(X509_REVOKED) *X509_CRL_get_REVOKED(X509_CRL *crl);

    const ASN1_INTEGER *X509_REVOKED_get0_serialNumber(const X509_REVOKED *r);
    const ASN1_TIME *X509_REVOKED_get0_revocationDate(const X509_REVOKED *r);

    int X509_REVOKED_set_serialNumber(X509_REVOKED *r, ASN1_INTEGER *serial);
    int X509_REVOKED_set_revocationDate(X509_REVOKED *r, ASN1_TIME *tm);

    int X509_CRL_add0_revoked(X509_CRL *crl, X509_REVOKED *rev);

    int X509_CRL_sort(X509_CRL *crl);

# DESCRIPTION

X509\_CRL\_get0\_by\_serial() attempts to find a revoked entry in **crl** for
serial number **serial**. If it is successful it sets **\*ret** to the internal
pointer of the matching entry, as a result **\*ret** must not be freed up
after the call.

X509\_CRL\_get0\_by\_cert() is similar to X509\_get0\_by\_serial() except it
looks for a revoked entry using the serial number of certificate **x**.

X509\_CRL\_get\_REVOKED() returns an internal pointer to a stack of all
revoked entries for **crl**.

X509\_REVOKED\_get0\_serialNumber() returns an internal pointer to the
serial number of **r**.

X509\_REVOKED\_get0\_revocationDate() returns an internal pointer to the
revocation date of **r**.

X509\_REVOKED\_set\_serialNumber() sets the serial number of **r** to **serial**.
The supplied **serial** pointer is not used internally so it should be
freed up after use.

X509\_REVOKED\_set\_revocationDate() sets the revocation date of **r** to
**tm**. The supplied **tm** pointer is not used internally so it should be
freed up after use.

X509\_CRL\_add0\_revoked() appends revoked entry **rev** to CRL **crl**. The
pointer **rev** is used internally so it must not be freed up after the call:
it is freed when the parent CRL is freed.

X509\_CRL\_sort() sorts the revoked entries of **crl** into ascending serial
number order.

# NOTES

Applications can determine the number of revoked entries returned by
X509\_CRL\_get\_revoked() using sk\_X509\_REVOKED\_num() and examine each one
in turn using sk\_X509\_REVOKED\_value().

# RETURN VALUES

X509\_CRL\_get0\_by\_serial(), X509\_CRL\_get0\_by\_cert(),
X509\_REVOKED\_set\_serialNumber(), X509\_REVOKED\_set\_revocationDate(),
X509\_CRL\_add0\_revoked() and X509\_CRL\_sort() return 1 for success and 0 for
failure.

X509\_REVOKED\_get0\_serialNumber() returns an **ASN1\_INTEGER** pointer.

X509\_REVOKED\_get0\_revocationDate() returns an **ASN1\_TIME** value.

X509\_CRL\_get\_REVOKED() returns a STACK of revoked entries.

# SEE ALSO

[d2i\_X509(3)](http://man.he.net/man3/d2i_X509),
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[X509\_get0\_signature(3)](http://man.he.net/man3/X509_get0_signature),
[X509\_get\_ext\_d2i(3)](http://man.he.net/man3/X509_get_ext_d2i),
[X509\_get\_extension\_flags(3)](http://man.he.net/man3/X509_get_extension_flags),
[X509\_get\_pubkey(3)](http://man.he.net/man3/X509_get_pubkey),
[X509\_get\_subject\_name(3)](http://man.he.net/man3/X509_get_subject_name),
[X509\_get\_version(3)](http://man.he.net/man3/X509_get_version),
[X509\_NAME\_add\_entry\_by\_txt(3)](http://man.he.net/man3/X509_NAME_add_entry_by_txt),
[X509\_NAME\_ENTRY\_get\_object(3)](http://man.he.net/man3/X509_NAME_ENTRY_get_object),
[X509\_NAME\_get\_index\_by\_NID(3)](http://man.he.net/man3/X509_NAME_get_index_by_NID),
[X509\_NAME\_print\_ex(3)](http://man.he.net/man3/X509_NAME_print_ex),
[X509\_new(3)](http://man.he.net/man3/X509_new),
[X509\_sign(3)](http://man.he.net/man3/X509_sign),
[X509V3\_get\_d2i(3)](http://man.he.net/man3/X509V3_get_d2i),
[X509\_verify\_cert(3)](http://man.he.net/man3/X509_verify_cert)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
