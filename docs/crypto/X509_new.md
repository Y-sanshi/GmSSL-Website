# NAME

X509\_chain\_up\_ref,
X509\_new, X509\_free, X509\_up\_ref - X509 certificate ASN1 allocation functions

# SYNOPSIS

    #include <openssl/x509.h>

    X509 *X509_new(void);
    void X509_free(X509 *a);
    int X509_up_ref(X509 *a);
    STACK_OF(X509) *X509_chain_up_ref(STACK_OF(X509) *x);

# DESCRIPTION

The X509 ASN1 allocation routines, allocate and free an
X509 structure, which represents an X509 certificate.

X509\_new() allocates and initializes a X509 structure with reference count
**1**.

X509\_free() decrements the reference count of **X509** structure **a** and
frees it up if the reference count is zero. If **a** is NULL nothing is done.

X509\_up\_ref() increments the reference count of **a**.

X509\_chain\_up\_ref() increases the reference count of all certificates in
chain **x** and returns a copy of the stack.

# NOTES

The function X509\_up\_ref() if useful if a certificate structure is being
used by several different operations each of which will free it up after
use: this avoids the need to duplicate the entire certificate structure.

The function X509\_chain\_up\_ref() doesn't just up the reference count of
each certificate it also returns a copy of the stack, using sk\_X509\_dup(),
but it serves a similar purpose: the returned chain persists after the
original has been freed.

# RETURN VALUES

If the allocation fails, X509\_new() returns **NULL** and sets an error
code that can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).
Otherwise it returns a pointer to the newly allocated structure.

X509\_up\_ref() returns 1 for success and 0 for failure.

X509\_chain\_up\_ref() returns a copy of the stack or **NULL** if an error
occurred.

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
[X509\_sign(3)](http://man.he.net/man3/X509_sign),
[X509V3\_get\_d2i(3)](http://man.he.net/man3/X509V3_get_d2i),
[X509\_verify\_cert(3)](http://man.he.net/man3/X509_verify_cert)

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
