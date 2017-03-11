# NAME

BUF\_MEM\_new, BUF\_MEM\_new\_ex, BUF\_MEM\_free, BUF\_MEM\_grow
BUF\_MEM\_grow\_clean, BUF\_reverse
\- simple character array structure

standard C library equivalents

# SYNOPSIS

    #include <openssl/buffer.h>

    BUF_MEM *BUF_MEM_new(void);

    BUF_MEM *BUF_MEM_new_ex(unsigned long flags);

    void BUF_MEM_free(BUF_MEM *a);

    int BUF_MEM_grow(BUF_MEM *str, int len);
    size_t BUF_MEM_grow_clean(BUF_MEM *str, size_t len);

    void BUF_reverse(unsigned char *out, const unsigned char *in, size_t size);

# DESCRIPTION

The buffer library handles simple character arrays. Buffers are used for
various purposes in the library, most notably memory BIOs.

BUF\_MEM\_new() allocates a new buffer of zero size.

BUF\_MEM\_new\_ex() allocates a buffer with the specified flags.
The flag **BUF\_MEM\_FLAG\_SECURE** specifies that the **data** pointer
should be allocated on the secure heap; see [CRYPTO\_secure\_malloc(3)](http://man.he.net/man3/CRYPTO_secure_malloc).

BUF\_MEM\_free() frees up an already existing buffer. The data is zeroed
before freeing up in case the buffer contains sensitive data.

BUF\_MEM\_grow() changes the size of an already existing buffer to
**len**. Any data already in the buffer is preserved if it increases in
size.

BUF\_MEM\_grow\_clean() is similar to BUF\_MEM\_grow() but it sets any free'd
or additionally-allocated memory to zero.

BUF\_reverse() reverses **size** bytes at **in** into **out**.  If **in**
is NULL, the array is reversed in-place.

# RETURN VALUES

BUF\_MEM\_new() returns the buffer or NULL on error.

BUF\_MEM\_free() has no return value.

BUF\_MEM\_grow() and BUF\_MEM\_grow\_clean() return
zero on error or the new size (i.e., **len**).

# SEE ALSO

[bio(7)](http://man.he.net/man7/bio),
[CRYPTO\_secure\_malloc(3)](http://man.he.net/man3/CRYPTO_secure_malloc).

# HISTORY

BUF\_MEM\_new\_ex() was added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
