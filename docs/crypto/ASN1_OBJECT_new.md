# NAME

ASN1\_OBJECT\_new, ASN1\_OBJECT\_free - object allocation functions

# SYNOPSIS

    #include <openssl/asn1.h>

    ASN1_OBJECT *ASN1_OBJECT_new(void);
    void ASN1_OBJECT_free(ASN1_OBJECT *a);

# DESCRIPTION

The ASN1\_OBJECT allocation routines, allocate and free an
ASN1\_OBJECT structure, which represents an ASN1 OBJECT IDENTIFIER.

ASN1\_OBJECT\_new() allocates and initializes an ASN1\_OBJECT structure.

ASN1\_OBJECT\_free() frees up the **ASN1\_OBJECT** structure **a**.
If **a** is NULL, nothing is done.

# NOTES

Although ASN1\_OBJECT\_new() allocates a new ASN1\_OBJECT structure it
is almost never used in applications. The ASN1 object utility functions
such as OBJ\_nid2obj() are used instead.

# RETURN VALUES

If the allocation fails, ASN1\_OBJECT\_new() returns **NULL** and sets an error
code that can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).
Otherwise it returns a pointer to the newly allocated structure.

ASN1\_OBJECT\_free() returns no value.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [d2i\_ASN1\_OBJECT(3)](http://man.he.net/man3/d2i_ASN1_OBJECT)

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
