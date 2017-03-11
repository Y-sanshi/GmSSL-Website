# NAME

X509\_NAME\_ENTRY\_get\_object, X509\_NAME\_ENTRY\_get\_data,
X509\_NAME\_ENTRY\_set\_object, X509\_NAME\_ENTRY\_set\_data,
X509\_NAME\_ENTRY\_create\_by\_txt, X509\_NAME\_ENTRY\_create\_by\_NID,
X509\_NAME\_ENTRY\_create\_by\_OBJ - X509\_NAME\_ENTRY utility functions

# SYNOPSIS

    #include <openssl/x509.h>

    ASN1_OBJECT * X509_NAME_ENTRY_get_object(const X509_NAME_ENTRY *ne);
    ASN1_STRING * X509_NAME_ENTRY_get_data(const X509_NAME_ENTRY *ne);

    int X509_NAME_ENTRY_set_object(X509_NAME_ENTRY *ne, const ASN1_OBJECT *obj);
    int X509_NAME_ENTRY_set_data(X509_NAME_ENTRY *ne, int type, const unsigned char *bytes, int len);

    X509_NAME_ENTRY *X509_NAME_ENTRY_create_by_txt(X509_NAME_ENTRY **ne, const char *field, int type, const unsigned char *bytes, int len);
    X509_NAME_ENTRY *X509_NAME_ENTRY_create_by_NID(X509_NAME_ENTRY **ne, int nid, int type, const unsigned char *bytes, int len);
    X509_NAME_ENTRY *X509_NAME_ENTRY_create_by_OBJ(X509_NAME_ENTRY **ne, const ASN1_OBJECT *obj, int type, const unsigned char *bytes, int len);

# DESCRIPTION

X509\_NAME\_ENTRY\_get\_object() retrieves the field name of **ne** in
and **ASN1\_OBJECT** structure.

X509\_NAME\_ENTRY\_get\_data() retrieves the field value of **ne** in
and **ASN1\_STRING** structure.

X509\_NAME\_ENTRY\_set\_object() sets the field name of **ne** to **obj**.

X509\_NAME\_ENTRY\_set\_data() sets the field value of **ne** to string type
**type** and value determined by **bytes** and **len**.

X509\_NAME\_ENTRY\_create\_by\_txt(), X509\_NAME\_ENTRY\_create\_by\_NID()
and X509\_NAME\_ENTRY\_create\_by\_OBJ() create and return an
**X509\_NAME\_ENTRY** structure.

# NOTES

X509\_NAME\_ENTRY\_get\_object() and X509\_NAME\_ENTRY\_get\_data() can be
used to examine an **X509\_NAME\_ENTRY** function as returned by
X509\_NAME\_get\_entry() for example.

X509\_NAME\_ENTRY\_create\_by\_txt(), X509\_NAME\_ENTRY\_create\_by\_NID(),
and X509\_NAME\_ENTRY\_create\_by\_OBJ() create and return an

X509\_NAME\_ENTRY\_create\_by\_txt(), X509\_NAME\_ENTRY\_create\_by\_OBJ(),
X509\_NAME\_ENTRY\_create\_by\_NID() and X509\_NAME\_ENTRY\_set\_data()
are seldom used in practice because **X509\_NAME\_ENTRY** structures
are almost always part of **X509\_NAME** structures and the
corresponding **X509\_NAME** functions are typically used to
create and add new entries in a single operation.

The arguments of these functions support similar options to the similarly
named ones of the corresponding **X509\_NAME** functions such as
X509\_NAME\_add\_entry\_by\_txt(). So for example **type** can be set to
**MBSTRING\_ASC** but in the case of X509\_set\_data() the field name must be
set first so the relevant field information can be looked up internally.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [d2i\_X509\_NAME(3)](http://man.he.net/man3/d2i_X509_NAME),
[OBJ\_nid2obj(3)](http://man.he.net/man3/OBJ_nid2obj)

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
