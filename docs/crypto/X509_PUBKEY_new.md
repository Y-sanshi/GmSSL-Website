# NAME

X509\_PUBKEY\_new, X509\_PUBKEY\_free, X509\_PUBKEY\_set, X509\_PUBKEY\_get0,
X509\_PUBKEY\_get, d2i\_PUBKEY, i2d\_PUBKEY, d2i\_PUBKEY\_bio, d2i\_PUBKEY\_fp,
i2d\_PUBKEY\_fp, i2d\_PUBKEY\_bio, X509\_PUBKEY\_set0\_param,
X509\_PUBKEY\_get0\_param - SubjectPublicKeyInfo public key functions

# SYNOPSIS

    #include <openssl/x509.h>

    X509_PUBKEY *X509_PUBKEY_new(void);
    void X509_PUBKEY_free(X509_PUBKEY *a);

    int X509_PUBKEY_set(X509_PUBKEY **x, EVP_PKEY *pkey);
    EVP_PKEY *X509_PUBKEY_get0(X509_PUBKEY *key);
    EVP_PKEY *X509_PUBKEY_get(X509_PUBKEY *key);

    EVP_PKEY *d2i_PUBKEY(EVP_PKEY **a, const unsigned char **pp, long length);
    int i2d_PUBKEY(EVP_PKEY *a, unsigned char **pp);

    EVP_PKEY *d2i_PUBKEY_bio(BIO *bp, EVP_PKEY **a);
    EVP_PKEY *d2i_PUBKEY_fp(FILE *fp, EVP_PKEY **a);

    int i2d_PUBKEY_fp(FILE *fp, EVP_PKEY *pkey);
    int i2d_PUBKEY_bio(BIO *bp, EVP_PKEY *pkey);

    int X509_PUBKEY_set0_param(X509_PUBKEY *pub, ASN1_OBJECT *aobj,
                               int ptype, void *pval,
                               unsigned char *penc, int penclen);
    int X509_PUBKEY_get0_param(ASN1_OBJECT **ppkalg,
                               const unsigned char **pk, int *ppklen,
                               X509_ALGOR **pa, X509_PUBKEY *pub);

# DESCRIPTION

The **X509\_PUBKEY** structure represents the ASN.1 **SubjectPublicKeyInfo**
structure defined in RFC5280 and used in certificates and certificate requests.

X509\_PUBKEY\_new() allocates and initializes an **X509\_PUBKEY** structure.

X509\_PUBKEY\_free() frees up **X509\_PUBKEY** structure **a**. If **a** is NULL
nothing is done.

X509\_PUBKEY\_set() sets the public key in **\*x** to the public key contained
in the **EVP\_PKEY** structure **pkey**. If **\*x** is not NULL any existing
public key structure will be freed.

X509\_PUBKEY\_get0() returns the public key contained in **key**. The returned
value is an internal pointer which **MUST NOT** be freed after use.

X509\_PUBKEY\_get() is similar to X509\_PUBKEY\_get0() except the reference
count on the returned key is incremented so it **MUST** be freed using
EVP\_PKEY\_free() after use.

d2i\_PUBKEY() and i2d\_PUBKEY() decode and encode an **EVP\_PKEY** structure
using **SubjectPublicKeyInfo** format. They otherwise follow the conventions of
other ASN.1 functions such as d2i\_X509().

d2i\_PUBKEY\_bio(), d2i\_PUBKEY\_fp(), i2d\_PUBKEY\_bio() and i2d\_PUBKEY\_fp() are
similar to d2i\_PUBKEY() and i2d\_PUBKEY() except they decode or encode using a
**BIO** or **FILE** pointer.

X509\_PUBKEY\_set0\_param() sets the public key parameters of **pub**. The
OID associated with the algorithm is set to **aobj**. The type of the
algorithm parameters is set to **type** using the structure **pval**.
The encoding of the public key itself is set to the **penclen**
bytes contained in buffer **penc**. On success ownership of all the supplied
parameters is passed to **pub** so they must not be freed after the
call.

X509\_PUBKEY\_get0\_param() retrieves the public key parameters from **pub**,
**\*ppkalg** is set to the associated OID and the encoding consists of
**\*ppklen** bytes at **\*pk**, **\*pa** is set to the associated
AlgorithmIdentifier for the public key. If the value of any of these
parameters is not required it can be set to **NULL**. All of the
retrieved pointers are internal and must not be freed after the
call.

# NOTES

The **X509\_PUBKEY** functions can be used to encode and decode public keys
in a standard format.

In many cases applications will not call the **X509\_PUBKEY** functions
directly: they will instead call wrapper functions such as X509\_get0\_pubkey().

# RETURN VALUES

If the allocation fails, X509\_PUBKEY\_new() returns **NULL** and sets an error
code that can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

Otherwise it returns a pointer to the newly allocated structure.

X509\_PUBKEY\_free() does not return a value.

X509\_PUBKEY\_get0() and X509\_PUBKEY\_get() return a pointer to an **EVP\_PKEY**
structure or **NULL** if an error occurs.

X509\_PUBKEY\_set(), X509\_PUBKEY\_set0\_param() and X509\_PUBKEY\_get0\_param()
return 1 for success and 0 if an error occurred.

# SEE ALSO

[d2i\_X509(3)](http://man.he.net/man3/d2i_X509),
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[X509\_get\_pubkey(3)](http://man.he.net/man3/X509_get_pubkey),

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
