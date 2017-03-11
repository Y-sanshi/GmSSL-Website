# NAME

X509\_ALGOR\_dup, X509\_ALGOR\_set0, X509\_ALGOR\_get0, X509\_ALGOR\_set\_md, X509\_ALGOR\_cmp - AlgorithmIdentifier functions

# SYNOPSIS

    #include <openssl/x509.h>

    X509_ALGOR *X509_ALGOR_dup(X509_ALGOR *alg);
    int X509_ALGOR_set0(X509_ALGOR *alg, ASN1_OBJECT *aobj, int ptype, void *pval);
    void X509_ALGOR_get0(const ASN1_OBJECT **paobj, int *pptype,
                         const void **ppval, const X509_ALGOR *alg);
    void X509_ALGOR_set_md(X509_ALGOR *alg, const EVP_MD *md);
    int X509_ALGOR_cmp(const X509_ALGOR *a, const X509_ALGOR *b);

# DESCRIPTION

X509\_ALGOR\_dup() returns a copy of **alg**.

X509\_ALGOR\_set0() sets the algorithm OID of **alg** to **aobj** and the
associated parameter type to **ptype** with value **pval**. If **ptype** is
**V\_ASN1\_UNDEF** the parameter is omitted, otherwise **ptype** and **pval** have
the same meaning as the **type** and **value** parameters to ASN1\_TYPE\_set().
All the supplied parameters are used internally so must **NOT** be freed after
this call.

X509\_ALGOR\_get0() is the inverse of X509\_ALGOR\_set0(): it returns the
algorithm OID in **\*paobj** and the associated parameter in **\*pptype**
and **\*ppval** from the **AlgorithmIdentifier** **alg**.

X509\_ALGOR\_set\_md() sets the **AlgorithmIdentifier** **alg** to appropriate
values for the message digest **md**.

X509\_ALGOR\_cmp() compares **a** and **b** and returns 0 if they have identical
encodings and non-zero otherwise.

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
