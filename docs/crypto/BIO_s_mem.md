# NAME

BIO\_s\_secmem,
BIO\_s\_mem, BIO\_set\_mem\_eof\_return, BIO\_get\_mem\_data, BIO\_set\_mem\_buf,
BIO\_get\_mem\_ptr, BIO\_new\_mem\_buf - memory BIO

# SYNOPSIS

    #include <openssl/bio.h>

    const BIO_METHOD *     BIO_s_mem(void);
    const BIO_METHOD *     BIO_s_secmem(void);

    BIO_set_mem_eof_return(BIO *b, int v)
    long BIO_get_mem_data(BIO *b, char **pp)
    BIO_set_mem_buf(BIO *b, BUF_MEM *bm, int c)
    BIO_get_mem_ptr(BIO *b, BUF_MEM **pp)

    BIO *BIO_new_mem_buf(const void *buf, int len);

# DESCRIPTION

BIO\_s\_mem() return the memory BIO method function.

A memory BIO is a source/sink BIO which uses memory for its I/O. Data
written to a memory BIO is stored in a BUF\_MEM structure which is extended
as appropriate to accommodate the stored data.

BIO\_s\_secmem() is like BIO\_s\_mem() except that the secure heap is used
for buffer storage.

Any data written to a memory BIO can be recalled by reading from it.
Unless the memory BIO is read only any data read from it is deleted from
the BIO.

Memory BIOs support BIO\_gets() and BIO\_puts().

If the BIO\_CLOSE flag is set when a memory BIO is freed then the underlying
BUF\_MEM structure is also freed.

Calling BIO\_reset() on a read write memory BIO clears any data in it if the
flag BIO\_FLAGS\_NONCLEAR\_RST is not set. On a read only BIO or if the flag
BIO\_FLAGS\_NONCLEAR\_RST is set it restores the BIO to its original state and
the data can be read again.

BIO\_eof() is true if no data is in the BIO.

BIO\_ctrl\_pending() returns the number of bytes currently stored.

BIO\_set\_mem\_eof\_return() sets the behaviour of memory BIO **b** when it is
empty. If the **v** is zero then an empty memory BIO will return EOF (that is
it will return zero and BIO\_should\_retry(b) will be false. If **v** is non
zero then it will return **v** when it is empty and it will set the read retry
flag (that is BIO\_read\_retry(b) is true). To avoid ambiguity with a normal
positive return value **v** should be set to a negative value, typically -1.

BIO\_get\_mem\_data() sets **pp** to a pointer to the start of the memory BIOs data
and returns the total amount of data available. It is implemented as a macro.

BIO\_set\_mem\_buf() sets the internal BUF\_MEM structure to **bm** and sets the
close flag to **c**, that is **c** should be either BIO\_CLOSE or BIO\_NOCLOSE.
It is a macro.

BIO\_get\_mem\_ptr() places the underlying BUF\_MEM structure in **pp**. It is
a macro.

BIO\_new\_mem\_buf() creates a memory BIO using **len** bytes of data at **buf**,
if **len** is -1 then the **buf** is assumed to be nul terminated and its
length is determined by **strlen**. The BIO is set to a read only state and
as a result cannot be written to. This is useful when some data needs to be
made available from a static area of memory in the form of a BIO. The
supplied data is read directly from the supplied buffer: it is **not** copied
first, so the supplied area of memory must be unchanged until the BIO is freed.

# NOTES

Writes to memory BIOs will always succeed if memory is available: that is
their size can grow indefinitely.

Every read from a read write memory BIO will remove the data just read with
an internal copy operation, if a BIO contains a lot of data and it is
read in small chunks the operation can be very slow. The use of a read only
memory BIO avoids this problem. If the BIO must be read write then adding
a buffering BIO to the chain will speed up the process.

Calling BIO\_set\_mem\_buf() on a BIO created with BIO\_new\_secmem() will
give undefined results, including perhaps a program crash.

# BUGS

There should be an option to set the maximum size of a memory BIO.

# EXAMPLE

Create a memory BIO and write some data to it:

    BIO *mem = BIO_new(BIO_s_mem());
    BIO_puts(mem, "Hello World\n");

Create a read only memory BIO:

    char data[] = "Hello World";
    BIO *mem;
    mem = BIO_new_mem_buf(data, -1);

Extract the BUF\_MEM structure from a memory BIO and then free up the BIO:

    BUF_MEM *bptr;
    BIO_get_mem_ptr(mem, &bptr);
    BIO_set_close(mem, BIO_NOCLOSE); /* So BIO_free() leaves BUF_MEM alone */
    BIO_free(mem);

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
