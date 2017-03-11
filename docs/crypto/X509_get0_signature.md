# NAME

X509\_get0\_signature, X509\_get\_signature\_nid, X509\_get0\_tbs\_sigalg,
X509\_REQ\_get0\_signature, X509\_REQ\_get\_signature\_nid, X509\_CRL\_get0\_signature,
X509\_CRL\_get\_signature\_nid - signature information

# SYNOPSIS

    #include <openssl/x509.h>

    void X509_get0_signature(const ASN1_BIT_STRING **psig,
                             const X509_ALGOR **palg,
                             const X509 *x);
    int X509_get_signature_nid(const X509 *x);
    const X509_ALGOR *X509_get0_tbs_sigalg(const X509 *x);

    void X509_REQ_get0_signature(const X509_REQ *crl,
                                 const ASN1_BIT_STRING **psig,
                                 const X509_ALGOR **palg);
    int X509_REQ_get_signature_nid(const X509_REQ *crl);

    void X509_CRL_get0_signature(const X509_CRL *crl,
                                 const ASN1_BIT_STRING **psig,
                                 const X509_ALGOR **palg);
    int X509_CRL_get_signature_nid(const X509_CRL *crl);

# DESCRIPTION

X509\_get0\_signature() sets **\*psig** to the signature of **x** and **\*palg**
to the signature algorithm of **x**. The values returned are internal
pointers which **MUST NOT** be freed up after the call.

X509\_get0\_tbs\_sigalg() returns the signature algorithm in the signed
portion of **x**.

X509\_get\_signature\_nid() returns the NID corresponding to the signature
algorithm of **x**.

X509\_REQ\_get0\_signature(), X509\_REQ\_get\_signature\_nid()
X509\_CRL\_get0\_signature() and X509\_CRL\_get\_signature\_nid() perform the
same function for certificate requests and CRLs.

# NOTES

These functions provide lower level access to signatures in certificates
where an application wishes to analyse or generate a signature in a form
where X509\_sign() et al is not appropriate (for example a non standard
or unsupported format).

# RETURN VALUES

X509\_get\_signature\_nid(), X509\_REQ\_get\_signature\_nid() and
X509\_CRL\_get\_signature\_nid() return a NID.

X509\_get0\_signature(), X509\_REQ\_get0\_signature() and
X509\_CRL\_get0\_signature() do not return values.

# SEE ALSO

[d2i\_X509(3)](http://man.he.net/man3/d2i_X509),
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[X509\_CRL\_get0\_by\_serial(3)](http://man.he.net/man3/X509_CRL_get0_by_serial),
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

# HISTORY

X509\_get0\_signature() and X509\_get\_signature\_nid() were first added to
OpenSSL 1.0.2.

X509\_REQ\_get0\_signature(), X509\_REQ\_get\_signature\_nid(),
X509\_CRL\_get0\_signature() and X509\_CRL\_get\_signature\_nid() were first added
to OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
