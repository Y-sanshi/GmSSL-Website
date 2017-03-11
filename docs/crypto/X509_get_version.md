# NAME

X509\_get\_version, X509\_set\_version, X509\_REQ\_get\_version, X509\_REQ\_set\_version,
X509\_CRL\_get\_version, X509\_CRL\_set\_version - get or set certificate,
certificate request or CRL version

# SYNOPSIS

    #include <openssl/x509.h>

    long X509_get_version(const X509 *x);
    int X509_set_version(X509 *x, long version);

    long X509_REQ_get_version(const X509_REQ *req);
    int X509_REQ_set_version(X509_REQ *x, long version);

    long X509_CRL_get_version(const X509_CRL *crl);
    int X509_CRL_set_version(X509_CRL *x, long version);

# DESCRIPTION

X509\_get\_version() returns the numerical value of the version field of
certificate **x**. Note: this is defined by standards (X.509 et al) to be one
less than the certificate version. So a version 3 certificate will return 2 and
a version 1 certificate will return 0.

X509\_set\_version() sets the numerical value of the version field of certificate
**x** to **version**.

Similarly X509\_REQ\_get\_version(), X509\_REQ\_set\_version(),
X509\_CRL\_get\_version() and X509\_CRL\_set\_version() get and set the version
number of certificate requests and CRLs.

# NOTES

The version field of certificates, certificate requests and CRLs has a
DEFAULT value of **v1(0)** meaning the field should be omitted for version
1\. This is handled transparently by these functions.

# RETURN VALUES

X509\_get\_version(), X509\_REQ\_get\_version() and X509\_CRL\_get\_version()
return the numerical value of the version field.

X509\_set\_version(), X509\_REQ\_set\_version() and X509\_CRL\_set\_version()
return 1 for success and 0 for failure.

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

X509\_get\_version(), X509\_REQ\_get\_version() and X509\_CRL\_get\_version() are
functions in OpenSSL 1.1.0, in previous versions they were macros.

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
