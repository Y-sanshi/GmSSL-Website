# NAME

X509\_get\_pubkey, X509\_get0\_pubkey, X509\_set\_pubkey, X509\_get\_X509\_PUBKEY,
X509\_REQ\_get\_pubkey, X509\_REQ\_get0\_pubkey, X509\_REQ\_set\_pubkey,
X509\_REQ\_get\_X509\_PUBKEY - get or set certificate or certificate request
public key

# SYNOPSIS

    #include <openssl/x509.h>

    EVP_PKEY *X509_get_pubkey(X509 *x);
    EVP_PKEY *X509_get0_pubkey(const X509 *x);
    int X509_set_pubkey(X509 *x, EVP_PKEY *pkey);
    X509_PUBKEY *X509_get_X509_PUBKEY(X509 *x);

    EVP_PKEY *X509_REQ_get_pubkey(X509_REQ *req);
    EVP_PKEY *X509_REQ_get0_pubkey(X509_REQ *req);
    int X509_REQ_set_pubkey(X509_REQ *x, EVP_PKEY *pkey);
    X509_PUBKEY *X509_REQ_get_X509_PUBKEY(X509_REQ *x);

# DESCRIPTION

X509\_get\_pubkey() attempts to decode the public key for certificate **x**. If
successful it returns the public key as an **EVP\_PKEY** pointer with its
reference count incremented: this means the returned key must be freed up
after use. X509\_get0\_pubkey() is similar except it does **not** increment
the reference count of the returned **EVP\_PKEY** so it must not be freed up
after use.

X509\_get\_X509\_PUBKEY() returns an internal pointer to the **X509\_PUBKEY**
structure which encodes the certificate of **x**. The returned value
must not be freed up after use.

X509\_set\_pubkey() attempts to set the public key for certificate **x** to
**pkey**. The key **pkey** should be freed up after use.

X509\_REQ\_get\_pubkey(), X509\_REQ\_get0\_pubkey(), X509\_REQ\_set\_pubkey() and
X509\_REQ\_get\_X509\_PUBKEY() are similar but operate on certificate request **req**.

# NOTES

The first time a public key is decoded the **EVP\_PKEY** structure is
cached in the certificate or certificate request itself. Subsequent calls
return the cached structure with its reference count incremented to
improve performance.

# RETURN VALUES

X509\_get\_pubkey(), X509\_get0\_pubkey(), X509\_get\_X509\_PUBKEY(),
X509\_REQ\_get\_pubkey() and X509\_REQ\_get\_X509\_PUBKEY() return a public key or
**NULL** if an error occurred.

X509\_set\_pubkey() and X509\_REQ\_set\_pubkey() return 1 for success and 0
for failure.

# SEE ALSO

[d2i\_X509(3)](http://man.he.net/man3/d2i_X509),
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[X509\_CRL\_get0\_by\_serial(3)](http://man.he.net/man3/X509_CRL_get0_by_serial),
[X509\_get0\_signature(3)](http://man.he.net/man3/X509_get0_signature),
[X509\_get\_ext\_d2i(3)](http://man.he.net/man3/X509_get_ext_d2i),
[X509\_get\_extension\_flags(3)](http://man.he.net/man3/X509_get_extension_flags),
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
