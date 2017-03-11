# NAME

X509\_get0\_notBefore, X509\_getm\_notBefore, X509\_get0\_notAfter,
X509\_getm\_notAfter, X509\_set1\_notBefore, X509\_set1\_notAfter,
X509\_CRL\_get0\_lastUpdate, X509\_CRL\_get0\_nextUpdate, X509\_CRL\_set1\_lastUpdate,
X509\_CRL\_set1\_nextUpdate - get or set certificate or CRL dates

# SYNOPSIS

    #include <openssl/x509.h>

    const ASN1_TIME *X509_get0_notBefore(const X509 *x);
    const ASN1_TIME *X509_get0_notAfter(const X509 *x);

    ASN1_TIME *X509_getm_notBefore(const X509 *x);
    ASN1_TIME *X509_getm_notAfter(const X509 *x);

    int X509_set1_notBefore(X509 *x, const ASN1_TIME *tm);
    int X509_set1_notAfter(X509 *x, const ASN1_TIME *tm);

    const ASN1_TIME *X509_CRL_get0_lastUpdate(const X509_CRL *crl);
    const ASN1_TIME *X509_CRL_get0_nextUpdate(const X509_CRL *crl);

    int X509_CRL_set1_lastUpdate(X509_CRL *x, const ASN1_TIME *tm);
    int X509_CRL_set1_nextUpdate(X509_CRL *x, const ASN1_TIME *tm);

# DESCRIPTION

X509\_get0\_notBefore() and X509\_get0\_notAfter() return the **notBefore**
and **notAfter** fields of certificate **x** respectively. The value
returned is an internal pointer which must not be freed up after
the call.

X509\_getm\_notBefore() and X509\_getm\_notAfter() are similar to
X509\_get0\_notBefore() and X509\_get0\_notAfter() except they return
non-constant mutable references to the associated date field of
the certficate.

X509\_set1\_notBefore() and X509\_set1\_notAfter() set the **notBefore**
and **notAfter** fields of **x** to **tm**. Ownership of the passed
parameter **tm** is not transferred by these functions so it must
be freed up after the call.

X509\_CRL\_get0\_lastUpdate() and X509\_CRL\_get0\_nextUpdate() return the
**lastUpdate** and **nextUpdate** fields of **crl**. The value
returned is an internal pointer which must not be freed up after
the call. If the **nextUpdate** field is absent from **crl** then
**NULL** is returned.

X509\_CRL\_set1\_lastUpdate() and X509\_CRL\_set1\_nextUpdate() set the **lastUpdate**
and **nextUpdate** fields of **crl** to **tm**. Ownership of the passed parameter
**tm** is not transferred by these functions so it must be freed up after the
call.

# RETURN VALUES

X509\_get0\_notBefore(), X509\_get0\_notAfter() and X509\_CRL\_get0\_lastUpdate()
return a pointer to an **ASN1\_TIME** structure.

X509\_CRL\_get0\_lastUpdate() return a pointer to an **ASN1\_TIME** structure
or NULL if the **lastUpdate** field is absent.

X509\_set1\_notBefore(), X509\_set1\_notAfter(), X509\_CRL\_set1\_lastUpdate() and
X509\_CRL\_set1\_nextUpdate() return 1 for success or 0 for failure.

# SEE ALSO

[d2i\_X509(3)](http://man.he.net/man3/d2i_X509),
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[X509\_CRL\_get0\_by\_serial(3)](http://man.he.net/man3/X509_CRL_get0_by_serial),
[X509\_get0\_signature(3)](http://man.he.net/man3/X509_get0_signature),
[X509\_get\_ext\_d2i(3)](http://man.he.net/man3/X509_get_ext_d2i),
[X509\_get\_extension\_flags(3)](http://man.he.net/man3/X509_get_extension_flags),
[X509\_get\_pubkey(3)](http://man.he.net/man3/X509_get_pubkey),
[X509\_get\_subject\_name(3)](http://man.he.net/man3/X509_get_subject_name),
[X509\_NAME\_add\_entry\_by\_txt(3)](http://man.he.net/man3/X509_NAME_add_entry_by_txt),
[X509\_NAME\_ENTRY\_get\_object(3)](http://man.he.net/man3/X509_NAME_ENTRY_get_object),
[X509\_NAME\_get\_index\_by\_NID(3)](http://man.he.net/man3/X509_NAME_get_index_by_NID),
[X509\_NAME\_print\_ex(3)](http://man.he.net/man3/X509_NAME_print_ex),
[X509\_new(3)](http://man.he.net/man3/X509_new),
[X509\_sign(3)](http://man.he.net/man3/X509_sign),
[X509V3\_get\_d2i(3)](http://man.he.net/man3/X509V3_get_d2i),
[X509\_verify\_cert(3)](http://man.he.net/man3/X509_verify_cert)

# HISTORY

These functions are available in all versions of OpenSSL.

X509\_get\_notBefore() and X509\_get\_notAfter() were deprecated in OpenSSL
1.1.0

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
