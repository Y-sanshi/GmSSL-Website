# NAME

BIO\_new, BIO\_up\_ref, BIO\_free, BIO\_vfree, BIO\_free\_all,
BIO\_set - BIO allocation and freeing functions

# SYNOPSIS

    #include <openssl/bio.h>

    BIO *  BIO_new(const BIO_METHOD *type);
    int    BIO_set(BIO *a, const BIO_METHOD *type);
    int    BIO_up_ref(BIO *a);
    int    BIO_free(BIO *a);
    void   BIO_vfree(BIO *a);
    void   BIO_free_all(BIO *a);

# DESCRIPTION

The BIO\_new() function returns a new BIO using method **type**.

BIO\_up\_ref() increments the reference count associated with the BIO object.

BIO\_free() frees up a single BIO, BIO\_vfree() also frees up a single BIO
but it does not return a value.
If **a** is NULL nothing is done.
Calling BIO\_free() may also have some effect
on the underlying I/O structure, for example it may close the file being
referred to under certain circumstances. For more details see the individual
BIO\_METHOD descriptions.

BIO\_free\_all() frees up an entire BIO chain, it does not halt if an error
occurs freeing up an individual BIO in the chain.
If **a** is NULL nothing is done.

# RETURN VALUES

BIO\_new() returns a newly created BIO or NULL if the call fails.

BIO\_set(), BIO\_up\_ref() and BIO\_free() return 1 for success and 0 for failure.

BIO\_free\_all() and BIO\_vfree() do not return values.

# NOTES

If BIO\_free() is called on a BIO chain it will only free one BIO resulting
in a memory leak.

Calling BIO\_free\_all() on a single BIO has the same effect as calling BIO\_free()
on it other than the discarded return value.

# HISTORY

BIO\_set() was removed in OpenSSL 1.1.0 as BIO type is now opaque.

# EXAMPLE

Create a memory BIO:

    BIO *mem = BIO_new(BIO_s_mem());

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
