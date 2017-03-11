# NAME

X509\_sign, X509\_sign\_ctx, X509\_verify, X509\_REQ\_sign, X509\_REQ\_sign\_ctx,
X509\_REQ\_verify, X509\_CRL\_sign, X509\_CRL\_sign\_ctx, X509\_CRL\_verify -
sign or verify certificate, certificate request or CRL signature

# SYNOPSIS

    #include <openssl/x509.h>

    int X509_sign(X509 *x, EVP_PKEY *pkey, const EVP_MD *md);
    int X509_sign_ctx(X509 *x, EVP_MD_CTX *ctx);
    int X509_verify(X509 *a, EVP_PKEY *r);

    int X509_REQ_sign(X509_REQ *x, EVP_PKEY *pkey, const EVP_MD *md);
    int X509_REQ_sign_ctx(X509_REQ *x, EVP_MD_CTX *ctx);
    int X509_REQ_verify(X509_REQ *a, EVP_PKEY *r);

    int X509_CRL_sign(X509_CRL *x, EVP_PKEY *pkey, const EVP_MD *md);
    int X509_CRL_sign_ctx(X509_CRL *x, EVP_MD_CTX *ctx);
    int X509_CRL_verify(X509_CRL *a, EVP_PKEY *r);

# DESCRIPTION

X509\_sign() signs certificate **x** using private key **pkey** and message
digest **md** and sets the signature in **x**. X509\_sign\_ctx() also signs
certificate **x** but uses the parameters contained in digest context **ctx**.

X509\_verify() verifies the signature of certificate **x** using public key
**pkey**. Only the signature is checked: no other checks (such as certificate
chain validity) are performed.

X509\_REQ\_sign(), X509\_REQ\_sign\_ctx(), X509\_REQ\_verify(),
X509\_CRL\_sign(), X509\_CRL\_sign\_ctx() and X509\_CRL\_verify() sign and verify
certificate requests and CRLs respectively.

# NOTES

X509\_sign\_ctx() is used where the default parameters for the corresponding
public key and digest are not suitable. It can be used to sign keys using
RSA-PSS for example.

For efficiency reasons and to work around ASN.1 encoding issues the encoding
of the signed portion of a certificate, certificate request and CRL is cached
internally. If the signed portion of the structure is modified the encoding
is not always updated meaning a stale version is sometimes used. This is not
normally a problem because modifying the signed portion will invalidate the
signature and signing will always update the encoding.

# RETURN VALUES

X509\_sign(), X509\_sign\_ctx(), X509\_REQ\_sign(), X509\_REQ\_sign\_ctx(),
X509\_CRL\_sign() and X509\_CRL\_sign\_ctx() return the size of the signature
in bytes for success and zero for failure.

X509\_verify(), X509\_REQ\_verify() and X509\_CRL\_verify() return 1 if the
signature is valid and 0 if the signature check fails. If the signature
could not be checked at all because it was invalid or some other error
occurred then -1 is returned.

# SEE ALSO

[d2i\_X509(3)](http://man.he.net/man3/d2i_X509),
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[X509\_CRL\_get0\_by\_serial(3)](http://man.he.net/man3/X509_CRL_get0_by_serial),
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
[X509V3\_get\_d2i(3)](http://man.he.net/man3/X509V3_get_d2i),
[X509\_verify\_cert(3)](http://man.he.net/man3/X509_verify_cert)

# HISTORY

X509\_sign(), X509\_REQ\_sign() and X509\_CRL\_sign() are available in all
versions of OpenSSL.

X509\_sign\_ctx(), X509\_REQ\_sign\_ctx() and X509\_CRL\_sign\_ctx() were first added
to OpenSSL 1.0.1.

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
