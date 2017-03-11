# NAME

ASN1\_STRING\_new, ASN1\_STRING\_type\_new, ASN1\_STRING\_free -
ASN1\_STRING allocation functions

# SYNOPSIS

    #include <openssl/asn1.h>

    ASN1_STRING * ASN1_STRING_new(void);
    ASN1_STRING * ASN1_STRING_type_new(int type);
    void ASN1_STRING_free(ASN1_STRING *a);

# DESCRIPTION

ASN1\_STRING\_new() returns an allocated **ASN1\_STRING** structure. Its type
is undefined.

ASN1\_STRING\_type\_new() returns an allocated **ASN1\_STRING** structure of
type **type**.

ASN1\_STRING\_free() frees up **a**.
If **a** is NULL nothing is done.

# NOTES

Other string types call the **ASN1\_STRING** functions. For example
ASN1\_OCTET\_STRING\_new() calls ASN1\_STRING\_type(V\_ASN1\_OCTET\_STRING).

# RETURN VALUES

ASN1\_STRING\_new() and ASN1\_STRING\_type\_new() return a valid
ASN1\_STRING structure or **NULL** if an error occurred.

ASN1\_STRING\_free() does not return a value.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error)

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
