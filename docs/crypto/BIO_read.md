# NAME

BIO\_read, BIO\_write, BIO\_gets, BIO\_puts - BIO I/O functions

# SYNOPSIS

    #include <openssl/bio.h>

    int    BIO_read(BIO *b, void *buf, int len);
    int    BIO_gets(BIO *b, char *buf, int size);
    int    BIO_write(BIO *b, const void *buf, int len);
    int    BIO_puts(BIO *b, const char *buf);

# DESCRIPTION

BIO\_read() attempts to read **len** bytes from BIO **b** and places
the data in **buf**.

BIO\_gets() performs the BIOs "gets" operation and places the data
in **buf**. Usually this operation will attempt to read a line of data
from the BIO of maximum length **len-1**. There are exceptions to this,
however; for example, BIO\_gets() on a digest BIO will calculate and
return the digest and other BIOs may not support BIO\_gets() at all.
The returned string is always NUL-terminated.

BIO\_write() attempts to write **len** bytes from **buf** to BIO **b**.

BIO\_puts() attempts to write a NUL-terminated string **buf** to BIO **b**.

# RETURN VALUES

All these functions return either the amount of data successfully read or
written (if the return value is positive) or that no data was successfully
read or written if the result is 0 or -1. If the return value is -2 then
the operation is not implemented in the specific BIO type.  The trailing
NUL is not included in the length returned by BIO\_gets().

# NOTES

A 0 or -1 return is not necessarily an indication of an error. In
particular when the source/sink is non-blocking or of a certain type
it may merely be an indication that no data is currently available and that
the application should retry the operation later.

One technique sometimes used with blocking sockets is to use a system call
(such as select(), poll() or equivalent) to determine when data is available
and then call read() to read the data. The equivalent with BIOs (that is call
select() on the underlying I/O structure and then call BIO\_read() to
read the data) should **not** be used because a single call to BIO\_read()
can cause several reads (and writes in the case of SSL BIOs) on the underlying
I/O structure and may block as a result. Instead select() (or equivalent)
should be combined with non blocking I/O so successive reads will request
a retry instead of blocking.

See [BIO\_should\_retry(3)](http://man.he.net/man3/BIO_should_retry) for details of how to
determine the cause of a retry and other I/O issues.

If the BIO\_gets() function is not supported by a BIO then it possible to
work around this by adding a buffering BIO [BIO\_f\_buffer(3)](http://man.he.net/man3/BIO_f_buffer)
to the chain.

# SEE ALSO

[BIO\_should\_retry(3)](http://man.he.net/man3/BIO_should_retry)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
