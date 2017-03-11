# NAME

ASN1\_TYPE\_get, ASN1\_TYPE\_set, ASN1\_TYPE\_set1, ASN1\_TYPE\_cmp, ASN1\_TYPE\_unpack\_sequence, ASN1\_TYPE\_pack\_sequence - ASN1\_TYPE utility
functions

# SYNOPSIS

    #include <openssl/asn1.h>

    int ASN1_TYPE_get(const ASN1_TYPE *a);
    void ASN1_TYPE_set(ASN1_TYPE *a, int type, void *value);
    int ASN1_TYPE_set1(ASN1_TYPE *a, int type, const void *value);
    int ASN1_TYPE_cmp(const ASN1_TYPE *a, const ASN1_TYPE *b);

    void *ASN1_TYPE_unpack_sequence(const ASN1_ITEM *it, const ASN1_TYPE *t);
    ASN1_TYPE *ASN1_TYPE_pack_sequence(const ASN1_ITEM *it, void *s,
                                       ASN1_TYPE **t);

# DESCRIPTION

These functions allow an ASN1\_TYPE structure to be manipulated. The
ASN1\_TYPE structure can contain any ASN.1 type or constructed type
such as a SEQUENCE: it is effectively equivalent to the ASN.1 ANY type.

ASN1\_TYPE\_get() returns the type of **a**.

ASN1\_TYPE\_set() sets the value of **a** to **type** and **value**. This
function uses the pointer **value** internally so it must **not** be freed
up after the call.

ASN1\_TYPE\_set1() sets the value of **a** to **type** a copy of **value**.

ASN1\_TYPE\_cmp() compares ASN.1 types **a** and **b** and returns 0 if
they are identical and non-zero otherwise.

ASN1\_TYPE\_unpack\_sequence() attempts to parse the SEQUENCE present in
**t** using the ASN.1 structure **it**. If successful it returns a pointer
to the ASN.1 structure corresponding to **it** which must be freed by the
caller. If it fails it return NULL.

ASN1\_TYPE\_pack\_sequence() attempts to encode the ASN.1 structure **s**
corresponding to **it** into an ASN1\_TYPE. If successful the encoded
ASN1\_TYPE is returned. If **t** and **\*t** are not NULL the encoded type
is written to **t** overwriting any existing data. If **t** is not NULL
but **\*t** is NULL the returned ASN1\_TYPE is written to **\*t**.

# NOTES

The type and meaning of the **value** parameter for ASN1\_TYPE\_set() and
ASN1\_TYPE\_set1() is determined by the **type** parameter.
If **type** is V\_ASN1\_NULL **value** is ignored. If **type** is V\_ASN1\_BOOLEAN
then the boolean is set to TRUE if **value** is not NULL. If **type** is
V\_ASN1\_OBJECT then value is an ASN1\_OBJECT structure. Otherwise **type**
is and ASN1\_STRING structure. If **type** corresponds to a primitive type
(or a string type) then the contents of the ASN1\_STRING contain the content
octets of the type. If **type** corresponds to a constructed type or
a tagged type (V\_ASN1\_SEQUENCE, V\_ASN1\_SET or V\_ASN1\_OTHER) then the
ASN1\_STRING contains the entire ASN.1 encoding verbatim (including tag and
length octets).

ASN1\_TYPE\_cmp() may not return zero if two types are equivalent but have
different encodings. For example the single content octet of the boolean TRUE
value under BER can have any non-zero encoding but ASN1\_TYPE\_cmp() will
only return zero if the values are the same.

If either or both of the parameters passed to ASN1\_TYPE\_cmp() is NULL the
return value is non-zero. Technically if both parameters are NULL the two
types could be absent OPTIONAL fields and so should match, however passing
NULL values could also indicate a programming error (for example an
unparseable type which returns NULL) for types which do **not** match. So
applications should handle the case of two absent values separately.

# RETURN VALUES

ASN1\_TYPE\_get() returns the type of the ASN1\_TYPE argument.

ASN1\_TYPE\_set() does not return a value.

ASN1\_TYPE\_set1() returns 1 for success and 0 for failure.

ASN1\_TYPE\_cmp() returns 0 if the types are identical and non-zero otherwise.

ASN1\_TYPE\_unpack\_sequence() returns a pointer to an ASN.1 structure or
NULL on failure.

ASN1\_TYPE\_pack\_sequence() return an ASN1\_TYPE structure if it succeeds or
NULL on failure.

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
