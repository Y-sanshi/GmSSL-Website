# NAME

X509v3\_get\_ext\_count, X509v3\_get\_ext, X509v3\_get\_ext\_by\_NID,
X509v3\_get\_ext\_by\_OBJ, X509v3\_get\_ext\_by\_critical, X509v3\_delete\_ext,
X509v3\_add\_ext, X509\_get\_ext\_count, X509\_get\_ext,
X509\_get\_ext\_by\_NID, X509\_get\_ext\_by\_OBJ, X509\_get\_ext\_by\_critical,
X509\_delete\_ext, X509\_add\_ext, X509\_CRL\_get\_ext\_count, X509\_CRL\_get\_ext,
X509\_CRL\_get\_ext\_by\_NID, X509\_CRL\_get\_ext\_by\_OBJ, X509\_CRL\_get\_ext\_by\_critical,
X509\_CRL\_delete\_ext, X509\_CRL\_add\_ext, X509\_REVOKED\_get\_ext\_count,
X509\_REVOKED\_get\_ext, X509\_REVOKED\_get\_ext\_by\_NID, X509\_REVOKED\_get\_ext\_by\_OBJ,
X509\_REVOKED\_get\_ext\_by\_critical, X509\_REVOKED\_delete\_ext,
X509\_REVOKED\_add\_ext - extension stack utility functions

# SYNOPSIS

    #include <openssl/x509.h>

    int X509v3_get_ext_count(const STACK_OF(X509_EXTENSION) *x);
    X509_EXTENSION *X509v3_get_ext(const STACK_OF(X509_EXTENSION) *x, int loc);

    int X509v3_get_ext_by_NID(const STACK_OF(X509_EXTENSION) *x,
                              int nid, int lastpos);
    int X509v3_get_ext_by_OBJ(const STACK_OF(X509_EXTENSION) *x,
                              const ASN1_OBJECT *obj, int lastpos);
    int X509v3_get_ext_by_critical(const STACK_OF(X509_EXTENSION) *x,
                                   int crit, int lastpos);
    X509_EXTENSION *X509v3_delete_ext(STACK_OF(X509_EXTENSION) *x, int loc);
    STACK_OF(X509_EXTENSION) *X509v3_add_ext(STACK_OF(X509_EXTENSION) **x,
                                             X509_EXTENSION *ex, int loc);

    int X509_get_ext_count(const X509 *x);
    X509_EXTENSION *X509_get_ext(const X509 *x, int loc);
    int X509_get_ext_by_NID(const X509 *x, int nid, int lastpos);
    int X509_get_ext_by_OBJ(const X509 *x, const ASN1_OBJECT *obj, int lastpos);
    int X509_get_ext_by_critical(const X509 *x, int crit, int lastpos);
    X509_EXTENSION *X509_delete_ext(X509 *x, int loc);
    int X509_add_ext(X509 *x, X509_EXTENSION *ex, int loc);

    int X509_CRL_get_ext_count(const X509_CRL *x);
    X509_EXTENSION *X509_CRL_get_ext(const X509_CRL *x, int loc);
    int X509_CRL_get_ext_by_NID(const X509_CRL *x, int nid, int lastpos);
    int X509_CRL_get_ext_by_OBJ(const X509_CRL *x, const ASN1_OBJECT *obj, int lastpos);
    int X509_CRL_get_ext_by_critical(const X509_CRL *x, int crit, int lastpos);
    X509_EXTENSION *X509_CRL_delete_ext(X509_CRL *x, int loc);
    int X509_CRL_add_ext(X509_CRL *x, X509_EXTENSION *ex, int loc);

    int X509_REVOKED_get_ext_count(const X509_REVOKED *x);
    X509_EXTENSION *X509_REVOKED_get_ext(const X509_REVOKED *x, int loc);
    int X509_REVOKED_get_ext_by_NID(const X509_REVOKED *x, int nid, int lastpos);
    int X509_REVOKED_get_ext_by_OBJ(const X509_REVOKED *x, const ASN1_OBJECT *obj,
                                   int lastpos);
    int X509_REVOKED_get_ext_by_critical(const X509_REVOKED *x, int crit, int lastpos);
    X509_EXTENSION *X509_REVOKED_delete_ext(X509_REVOKED *x, int loc);
    int X509_REVOKED_add_ext(X509_REVOKED *x, X509_EXTENSION *ex, int loc);

# DESCRIPTION

X509v3\_get\_ext\_count() retrieves the number of extensions in **x**.

X509v3\_get\_ext() retrieves extension **loc** from **x**. The index **loc**
can take any value from **0** to X509\_get\_ext\_count(x) - 1. The returned
extension is an internal pointer which **must not** be freed up by the
application.

X509v3\_get\_ext\_by\_NID() and X509v3\_get\_ext\_by\_OBJ() look for an extension
with **nid** or **obj** from extension stack **x**. The search starts from the
extension after **lastpos** or from the beginning if &lt;lastpos> is **-1**. If
the extension is found its index is returned otherwise **-1** is returned.

X509v3\_get\_ext\_by\_critical() is similar to X509v3\_get\_ext\_by\_NID() except it
looks for an extension of criticality **crit**. A zero value for **crit**
looks for a non-critical extension a non-zero value looks for a critical
extension.

X509v3\_delete\_ext() deletes the extension with index **loc** from **x**. The
deleted extension is returned and must be freed by the caller. If **loc**
is in invalid index value **NULL** is returned.

X509v3\_add\_ext() adds extension **ex** to stack **\*x** at position **loc**. If
**loc** is **-1** the new extension is added to the end. If **\*x** is **NULL**
a new stack will be allocated. The passed extension **ex** is duplicated
internally so it must be freed after use.

X509\_get\_ext\_count(), X509\_get\_ext(), X509\_get\_ext\_by\_NID(),
X509\_get\_ext\_by\_OBJ(), X509\_get\_ext\_by\_critical(), X509\_delete\_ext()
and X509\_add\_ext() operate on the extensions of certificate **x** they are
otherwise identical to the X509v3 functions.

X509\_CRL\_get\_ext\_count(), X509\_CRL\_get\_ext(), X509\_CRL\_get\_ext\_by\_NID(),
X509\_CRL\_get\_ext\_by\_OBJ(), X509\_CRL\_get\_ext\_by\_critical(),
X509\_CRL\_delete\_ext() and X509\_CRL\_add\_ext() operate on the extensions of
CRL **x** they are otherwise identical to the X509v3 functions.

X509\_REVOKED\_get\_ext\_count(), X509\_REVOKED\_get\_ext(),
X509\_REVOKED\_get\_ext\_by\_NID(), X509\_REVOKED\_get\_ext\_by\_OBJ(),
X509\_REVOKED\_get\_ext\_by\_critical(), X509\_REVOKED\_delete\_ext() and
X509\_REVOKED\_add\_ext() operate on the extensions of CRL entry **x**
they are otherwise identical to the X509v3 functions.

# NOTES

These functions are used to examine stacks of extensions directly. Many
applications will want to parse or encode and add an extension: they should
use the extension encode and decode functions instead such as
X509\_add1\_ext\_i2d() and X509\_get\_ext\_d2i().

Extension indices start from zero, so a zero index return value is **not** an
error. These search functions start from the extension **after** the **lastpos**
parameter so it should initially be set to **-1**, if it is set to zero the
initial extension will not be checked.

# RETURN VALUES

X509v3\_get\_ext\_count() returns the extension count.

X509v3\_get\_ext() and X509v3\_delete\_ext() return an **X509\_EXTENSION** pointer
or **NULL** if an error occurs.

X509v3\_get\_ext\_by\_NID() X509v3\_get\_ext\_by\_OBJ() and
X509v3\_get\_ext\_by\_critical() return the an extension index or **-1** if an
error occurs.

X509v3\_add\_ext() returns a stack of extensions or **NULL** on error.

# SEE ALSO

[X509V3\_get\_d2i(3)](http://man.he.net/man3/X509V3_get_d2i)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
