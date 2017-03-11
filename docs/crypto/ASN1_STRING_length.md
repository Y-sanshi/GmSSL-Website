# NAME

ASN1\_STRING\_dup, ASN1\_STRING\_cmp, ASN1\_STRING\_set, ASN1\_STRING\_length,
ASN1\_STRING\_type, ASN1\_STRING\_get0\_data, ASN1\_STRING\_data,
ASN1\_STRING\_to\_UTF8 - ASN1\_STRING utility functions

# SYNOPSIS

    #include <openssl/asn1.h>

    int ASN1_STRING_length(ASN1_STRING *x);
    const unsigned char * ASN1_STRING_get0_data(const ASN1_STRING *x);
    unsigned char * ASN1_STRING_data(ASN1_STRING *x);

    ASN1_STRING * ASN1_STRING_dup(ASN1_STRING *a);

    int ASN1_STRING_cmp(ASN1_STRING *a, ASN1_STRING *b);

    int ASN1_STRING_set(ASN1_STRING *str, const void *data, int len);

    int ASN1_STRING_type(const ASN1_STRING *x);

    int ASN1_STRING_to_UTF8(unsigned char **out, const ASN1_STRING *in);

# DESCRIPTION

These functions allow an **ASN1\_STRING** structure to be manipulated.

ASN1\_STRING\_length() returns the length of the content of **x**.

ASN1\_STRING\_get0\_data() returns an internal pointer to the data of **x**.
Since this is an internal pointer it should **not** be freed or
modified in any way.

ASN1\_STRING\_data() is similar to ASN1\_STRING\_get0\_data() except the
returned value is not constant. This function is deprecated:
applications should use ASN1\_STRING\_get0\_data() instead.

ASN1\_STRING\_dup() returns a copy of the structure **a**.

ASN1\_STRING\_cmp() compares **a** and **b** returning 0 if the two
are identical. The string types and content are compared.

ASN1\_STRING\_set() sets the data of string **str** to the buffer
**data** or length **len**. The supplied data is copied. If **len**
is -1 then the length is determined by strlen(data).

ASN1\_STRING\_type() returns the type of **x**, using standard constants
such as **V\_ASN1\_OCTET\_STRING**.

ASN1\_STRING\_to\_UTF8() converts the string **in** to UTF8 format, the
converted data is allocated in a buffer in **\*out**. The length of
**out** is returned or a negative error code. The buffer **\*out**
should be freed using OPENSSL\_free().

# NOTES

Almost all ASN1 types in OpenSSL are represented as an **ASN1\_STRING**
structure. Other types such as **ASN1\_OCTET\_STRING** are simply typedef'ed
to **ASN1\_STRING** and the functions call the **ASN1\_STRING** equivalents.
**ASN1\_STRING** is also used for some **CHOICE** types which consist
entirely of primitive string types such as **DirectoryString** and
**Time**.

These functions should **not** be used to examine or modify **ASN1\_INTEGER**
or **ASN1\_ENUMERATED** types: the relevant **INTEGER** or **ENUMERATED**
utility functions should be used instead.

In general it cannot be assumed that the data returned by ASN1\_STRING\_data()
is null terminated or does not contain embedded nulls. The actual format
of the data will depend on the actual string type itself: for example
for and IA5String the data will be ASCII, for a BMPString two bytes per
character in big endian format, UTF8String will be in UTF8 format.

Similar care should be take to ensure the data is in the correct format
when calling ASN1\_STRING\_set().

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error)

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
