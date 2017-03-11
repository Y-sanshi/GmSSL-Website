# NAME

DECLARE\_PEM\_rw,
PEM\_read\_CMS,
PEM\_read\_bio\_CMS,
PEM\_write\_CMS,
PEM\_write\_bio\_CMS,
PEM\_write\_DHxparams,
PEM\_write\_bio\_DHxparams,
PEM\_read\_ECPKParameters,
PEM\_read\_bio\_ECPKParameters,
PEM\_write\_ECPKParameters,
PEM\_write\_bio\_ECPKParameters,
PEM\_read\_ECPrivateKey,
PEM\_write\_ECPrivateKey,
PEM\_write\_bio\_ECPrivateKey,
PEM\_read\_EC\_PUBKEY,
PEM\_read\_bio\_EC\_PUBKEY,
PEM\_write\_EC\_PUBKEY,
PEM\_write\_bio\_EC\_PUBKEY,
PEM\_read\_NETSCAPE\_CERT\_SEQUENCE,
PEM\_read\_bio\_NETSCAPE\_CERT\_SEQUENCE,
PEM\_write\_NETSCAPE\_CERT\_SEQUENCE,
PEM\_write\_bio\_NETSCAPE\_CERT\_SEQUENCE,
PEM\_read\_PKCS8,
PEM\_read\_bio\_PKCS8,
PEM\_write\_PKCS8,
PEM\_write\_bio\_PKCS8,
PEM\_write\_PKCS8\_PRIV\_KEY\_INFO,
PEM\_read\_bio\_PKCS8\_PRIV\_KEY\_INFO,
PEM\_read\_PKCS8\_PRIV\_KEY\_INFO,
PEM\_write\_bio\_PKCS8\_PRIV\_KEY\_INFO,
PEM\_read\_SSL\_SESSION,
PEM\_read\_bio\_SSL\_SESSION,
PEM\_write\_SSL\_SESSION,
PEM\_write\_bio\_SSL\_SESSION
\- PEM object encoding routines

# SYNOPSIS

    #include <openssl/pem.h>

    DECLARE_PEM_rw(name, TYPE)

    TYPE *PEM_read_TYPE(FILE *fp, TYPE **a, pem_password_cb *cb, void *u);
    TYPE *PEM_read_bio_TYPE(BIO *bp, TYPE **a, pem_password_cb *cb, void *u);
    int PEM_write_TYPE(FILE *fp, const TYPE *a);
    int PEM_write_bio_TYPE(BIO *bp, const TYPE *a);

# DESCRIPTION

In the description below, _TYPE_ is used
as a placeholder for any of the OpenSSL datatypes, such as _X509_.
The macro **DECLARE\_PEM\_rw** expands to the set of declarations shown in
the next four lines of the synopsis.

These routines convert between local instances of ASN1 datatypes and
the PEM encoding.  For more information on the templates, see
[ASN1\_ITEM(3)](http://man.he.net/man3/ASN1_ITEM).  For more information on the lower-level routines used
by the functions here, see [PEM\_read(3)](http://man.he.net/man3/PEM_read).

PEM\_read\_TYPE() reads a PEM-encoded object of _TYPE_ from the file **fp**
and returns it.  The **cb** and **u** parameters are as described in
[pem\_password\_cb(3)](http://man.he.net/man3/pem_password_cb).

PEM\_read\_bio\_TYPE() is similar to PEM\_read\_TYPE() but reads from the BIO **bp**.

PEM\_write\_TYPE() writes the PEM encoding of the object **a** to the file **fp**.

PEM\_write\_bio\_TYPE() similarly writes to the BIO **bp**.

# RETURN VALUES

PEM\_read\_TYPE() and PEM\_read\_bio\_TYPE() return a pointer to an allocated
object, which should be released by calling TYPE\_free(), or NULL on error.

PEM\_write\_TYPE() and PEM\_write\_bio\_TYPE() return the number of bytes written
or zero on error.

# SEE ALSO

[PEM\_read(3)](http://man.he.net/man3/PEM_read)

# COPYRIGHT

Copyright 1998-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
