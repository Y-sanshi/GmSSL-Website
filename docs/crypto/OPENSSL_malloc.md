# NAME

OPENSSL\_malloc\_init,
OPENSSL\_malloc, OPENSSL\_zalloc, OPENSSL\_realloc, OPENSSL\_free,
OPENSSL\_clear\_realloc, OPENSSL\_clear\_free, OPENSSL\_cleanse,
CRYPTO\_malloc, CRYPTO\_zalloc, CRYPTO\_realloc, CRYPTO\_free,
OPENSSL\_strdup, OPENSSL\_strndup,
OPENSSL\_memdup, OPENSSL\_strlcpy, OPENSSL\_strlcat,
OPENSSL\_hexstr2buf, OPENSSL\_buf2hexstr, OPENSSL\_hexchar2int,
CRYPTO\_strdup, CRYPTO\_strndup,
OPENSSL\_mem\_debug\_push, OPENSSL\_mem\_debug\_pop,
CRYPTO\_mem\_debug\_push, CRYPTO\_mem\_debug\_pop,
CRYPTO\_clear\_realloc, CRYPTO\_clear\_free,
CRYPTO\_get\_mem\_functions, CRYPTO\_set\_mem\_functions,
CRYPTO\_set\_mem\_debug, CRYPTO\_mem\_ctrl,
CRYPTO\_mem\_leaks, CRYPTO\_mem\_leaks\_fp - Memory allocation functions

# SYNOPSIS

    #include <openssl/crypto.h>

    int OPENSSL_malloc_init(void)

    void *OPENSSL_malloc(size_t num)
    void *OPENSSL_zalloc(size_t num)
    void *OPENSSL_realloc(void *addr, size_t num)
    void OPENSSL_free(void *addr)
    char *OPENSSL_strdup(const char *str)
    char *OPENSSL_strndup(const char *str, size_t s)
    size_t OPENSSL_strlcat(char *dst, const char *src, size_t size);
    size_t OPENSSL_strlcpy(char *dst, const char *src, size_t size);
    void *OPENSSL_memdup(void *data, size_t s)
    void *OPENSSL_clear_realloc(void *p, size_t old_len, size_t num)
    void OPENSSL_clear_free(void *str, size_t num)
    void OPENSSL_cleanse(void *ptr, size_t len);

    unsigned char *OPENSSL_hexstr2buf(const char *str, long *len);
    char *OPENSSL_buf2hexstr(const unsigned char *buffer, long len);
    int OPENSSL_hexchar2int(unsigned char c);

    void *CRYPTO_malloc(size_t num, const char *file, int line)
    void *CRYPTO_zalloc(size_t num, const char *file, int line)
    void *CRYPTO_realloc(void *p, size_t num, const char *file, int line)
    void CRYPTO_free(void *str, const char *, int)
    char *CRYPTO_strdup(const char *p, const char *file, int line)
    char *CRYPTO_strndup(const char *p, size_t num, const char *file, int line)
    void *CRYPTO_clear_realloc(void *p, size_t old_len, size_t num, const char *file, int line)
    void CRYPTO_clear_free(void *str, size_t num, const char *, int)

    void CRYPTO_get_mem_functions(
            void *(**m)(size_t, const char *, int),
            void *(**r)(void *, size_t, const char *, int),
            void (**f)(void *, const char *, int))
    int CRYPTO_set_mem_functions(
            void *(*m)(size_t, const char *, int),
            void *(*r)(void *, size_t, const char *, int),
            void (*f)(void *, const char *, int))

    int CRYPTO_set_mem_debug(int onoff)

    int CRYPTO_mem_ctrl(int mode);

    int OPENSSL_mem_debug_push(const char *info)
    int OPENSSL_mem_debug_pop(void);

    int CRYPTO_mem_debug_push(const char *info, const char *file, int line);
    int CRYPTO_mem_debug_pop(void);

    void CRYPTO_mem_leaks(BIO *b);
    void CRYPTO_mem_leaks_fp(FILE *fp);

# DESCRIPTION

OpenSSL memory allocation is handled by the **OPENSSL\_xxx** API. These are
generally macro's that add the standard C **\_\_FILE\_\_** and **\_\_LINE\_\_**
parameters and call a lower-level **CRYPTO\_xxx** API.
Some functions do not add those parameters, but exist for consistency.

OPENSSL\_malloc\_init() sets the lower-level memory allocation functions
to their default implementation.
It is generally not necessary to call this, except perhaps in certain
shared-library situations.

OPENSSL\_malloc(), OPENSSL\_realloc(), and OPENSSL\_free() are like the
C malloc(), realloc(), and free() functions.
OPENSSL\_zalloc() calls memset() to zero the memory before returning.

OPENSSL\_clear\_realloc() and OPENSSL\_clear\_free() should be used
when the buffer at **addr** holds sensitive information.
The old buffer is filled with zero's by calling OPENSSL\_cleanse()
before ultimately calling OPENSSL\_free().

OPENSSL\_cleanse() fills **ptr** of size **len** with a string of 0's.
Use OPENSSL\_cleanse() with care if the memory is a mapping of a file.
If the storage controller uses write compression, then its possible
that sensitive tail bytes will survive zeroization because the block of
zeros will be compressed. If the storage controller uses wear leveling,
then the old sensitive data will not be overwritten; rather, a block of
0's will be written at a new physical location.

OPENSSL\_strdup(), OPENSSL\_strndup() and OPENSSL\_memdup() are like the
equivalent C functions, except that memory is allocated by calling the
OPENSSL\_malloc() and should be released by calling OPENSSL\_free().

OPENSSL\_strlcpy(),
OPENSSL\_strlcat() and OPENSSL\_strnlen() are equivalents of the common C
library functions and are provided for portability.

OPENSSL\_hexstr2buf() parses **str** as a hex string and returns a
pointer to the parsed value. The memory is allocated by calling
OPENSSL\_malloc() and should be released by calling OPENSSL\_free().
If **len** is not NULL, it is filled in with the output length.
Colons between two-character hex "bytes" are ignored.
An odd number of hex digits is an error.

OPENSSL\_buf2hexstr() takes the specified buffer and length, and returns
a hex string for value, or NULL on error.
**Buffer** cannot be NULL; if **len** is 0 an empty string is returned.

OPENSSL\_hexchar2int() converts a character to the hexadecimal equivalent,
or returns -1 on error.

If no allocations have been done, it is possible to "swap out" the default
implementations for OPENSSL\_malloc(), OPENSSL\_realloc and OPENSSL\_free()
and replace them with alternate versions (hooks).
CRYPTO\_get\_mem\_functions() function fills in the given arguments with the
function pointers for the current implementations.
With CRYPTO\_set\_mem\_functions(), you can specify a different set of functions.
If any of **m**, **r**, or **f** are NULL, then the function is not changed.

The default implementation can include some debugging capability (if enabled
at build-time).
This adds some overhead by keeping a list of all memory allocations, and
removes items from the list when they are free'd.
This is most useful for identifying memory leaks.
CRYPTO\_set\_mem\_debug() turns this tracking on and off.  In order to have
any effect, is must be called before any of the allocation functions
(e.g., CRYPTO\_malloc()) are called, and is therefore normally one of the
first lines of main() in an application.

CRYPTO\_mem\_ctrl() provides fine-grained control of memory leak tracking.
To enable tracking call CRYPTO\_mem\_ctrl() with a **mode** argument of
the **CRYPTO\_MEM\_CHECK\_ON**.
To disable tracking call CRYPTO\_mem\_ctrl() with a **mode** argument of
the **CRYPTO\_MEM\_CHECK\_OFF**.

While checking memory, it can be useful to store additional context
about what is being done.
For example, identifying the field names when parsing a complicated
data structure.
OPENSSL\_mem\_debug\_push() (which calls CRYPTO\_mem\_debug\_push())
attachs an identifying string to the allocation stack.
This must be a global or other static string; it is not copied.
OPENSSL\_mem\_debug\_pop() removes identifying state from the stack.

At the end of the program, calling CRYPTO\_mem\_leaks() or
CRYPTO\_mem\_leaks\_fp() will report all "leaked" memory, writing it
to the specified BIO **b** or FILE **fp**. These functions return 1 if
there are no leaks, 0 if there are leaks and -1 if an error occurred.

# RETURN VALUES

OPENSSL\_malloc\_init(), OPENSSL\_free(), OPENSSL\_clear\_free()
CRYPTO\_free(), CRYPTO\_clear\_free() and CRYPTO\_get\_mem\_functions()
return no value.

CRYPTO\_mem\_leaks() and CRYPTO\_mem\_leaks\_fp() return 1 if there
are no leaks, 0 if there are leaks and -1 if an error occurred.

OPENSSL\_malloc(), OPENSSL\_zalloc(), OPENSSL\_realloc(),
OPENSSL\_clear\_realloc(),
CRYPTO\_malloc(), CRYPTO\_zalloc(), CRYPTO\_realloc(),
CRYPTO\_clear\_realloc(),
OPENSSL\_buf2hexstr(), OPENSSL\_hexstr2buf(),
OPENSSL\_strdup(), and OPENSSL\_strndup()
return a pointer to allocated memory or NULL on error.

CRYPTO\_set\_mem\_functions() and CRYPTO\_set\_mem\_debug()
return 1 on success or 0 on failure (almost
always because allocations have already happened).

CRYPTO\_mem\_ctrl() returns -1 if an error occurred, otherwise the
previous value of the mode.

OPENSSL\_mem\_debug\_push() and OPENSSL\_mem\_debug\_pop()
return 1 on success or 0 on failure.

# NOTES

While it's permitted to swap out only a few and not all the functions
with CRYPTO\_set\_mem\_functions(), it's recommended to swap them all out
at once.  _This applies specially if OpenSSL was built with the
configuration option_ `crypto-mdebug` _enabled.  In case, swapping out
only, say, the malloc() implementation is outright dangerous._

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
