# NAME

CRYPTO\_secure\_malloc\_init, CRYPTO\_secure\_malloc\_initialized,
CRYPTO\_secure\_malloc\_done, OPENSSL\_secure\_malloc, CRYPTO\_secure\_malloc,
OPENSSL\_secure\_zalloc, CRYPTO\_secure\_zalloc, OPENSSL\_secure\_free,
CRYPTO\_secure\_free, OPENSSL\_secure\_actual\_size, OPENSSL\_secure\_allocated,
CRYPTO\_secure\_used - secure heap storage

# SYNOPSIS

    #include <openssl/crypto.h>

    int CRYPTO_secure_malloc_init(size_t size, int minsize);

    int CRYPTO_secure_malloc_initialized();

    int CRYPTO_secure_malloc_done();

    void *OPENSSL_secure_malloc(size_t num);
    void *CRYPTO_secure_malloc(size_t num, const char *file, int line);

    void *OPENSSL_secure_zalloc(size_t num);
    void *CRYPTO_secure_zalloc(size_t num, const char *file, int line);

    void OPENSSL_secure_free(void* ptr);
    void CRYPTO_secure_free(void *ptr, const char *, int);

    size_t OPENSSL_secure_actual_size(const void *ptr);
    int OPENSSL_secure_allocated(const void *ptr);

    size_t CRYPTO_secure_used();

# DESCRIPTION

In order to help protect applications (particularly long-running servers)
from pointer overruns or underruns that could return arbitrary data from
the program's dynamic memory area, where keys and other sensitive
information might be stored, OpenSSL supports the concept of a "secure heap."
The level and type of security guarantees depend on the operating system.
It is a good idea to review the code and see if it addresses your
threat model and concerns.

If a secure heap is used, then private key **BIGNUM** values are stored there.
This protects long-term storage of private keys, but will not necessarily
put all intermediate values and computations there.

CRYPTO\_secure\_malloc\_init() creates the secure heap, with the specified
`size` in bytes. The `minsize` parameter is the minimum size to
allocate from the heap. Both `size` and `minsize` must be a power
of two.

CRYPTO\_secure\_malloc\_initialized() indicates whether or not the secure
heap as been initialized and is available.

CRYPTO\_secure\_malloc\_done() releases the heap and makes the memory unavailable
to the process if all secure memory has been freed.
It can take noticeably long to complete.

OPENSSL\_secure\_malloc() allocates `num` bytes from the heap.
If CRYPTO\_secure\_malloc\_init() is not called, this is equivalent to
calling OPENSSL\_malloc().
It is a macro that expands to
CRYPTO\_secure\_malloc() and adds the `__FILE__` and `__LINE__` parameters.

OPENSSL\_secure\_zalloc() and CRYPTO\_secure\_zalloc() are like
OPENSSL\_secure\_malloc() and CRYPTO\_secure\_malloc(), respectively,
except that they call memset() to zero the memory before returning.

OPENSSL\_secure\_free() releases the memory at `ptr` back to the heap.
It must be called with a value previously obtained from
OPENSSL\_secure\_malloc().
If CRYPTO\_secure\_malloc\_init() is not called, this is equivalent to
calling OPENSSL\_free().
It exists for consistency with OPENSSL\_secure\_malloc() , and
is a macro that expands to CRYPTO\_secure\_free() and adds the `__FILE__`
and `__LINE__` parameters..

OPENSSL\_secure\_allocated() tells whether or not a pointer is within
the secure heap.
OPENSSL\_secure\_actual\_size() tells the actual size allocated to the
pointer; implementations may allocate more space than initially
requested, in order to "round up" and reduce secure heap fragmentation.

CRYPTO\_secure\_used() returns the number of bytes allocated in the
secure heap.

# RETURN VALUES

CRYPTO\_secure\_malloc\_init() returns 0 on failure, 1 if successful,
and 2 if successful but the heap could not be protected by memory
mapping.

CRYPTO\_secure\_malloc\_initialized() returns 1 if the secure heap is
available (that is, if CRYPTO\_secure\_malloc\_init() has been called,
but CRYPTO\_secure\_malloc\_done() has not been called or failed) or 0 if not.

OPENSSL\_secure\_malloc() and OPENSSL\_secure\_zalloc() return a pointer into
the secure heap of the requested size, or `NULL` if memory could not be
allocated.

CRYPTO\_secure\_allocated() returns 1 if the pointer is in the secure heap, or 0 if not.

CRYPTO\_secure\_malloc\_done() returns 1 if the secure memory area is released, or 0 if not.

OPENSSL\_secure\_free() returns no values.

# SEE ALSO

[OPENSSL\_malloc(3)](http://man.he.net/man3/OPENSSL_malloc),
[BN\_new(3)](http://man.he.net/man3/BN_new)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
