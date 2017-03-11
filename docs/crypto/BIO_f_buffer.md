# NAME

BIO\_get\_buffer\_num\_lines,
BIO\_set\_read\_buffer\_size,
BIO\_set\_write\_buffer\_size,
BIO\_set\_buffer\_size,
BIO\_set\_buffer\_read\_data,
BIO\_f\_buffer
\- buffering BIO

# SYNOPSIS

    #include <openssl/bio.h>

    const BIO_METHOD *BIO_f_buffer(void);

    long BIO_get_buffer_num_lines(BIO *b);
    long BIO_set_read_buffer_size(BIO *b, long size);
    long BIO_set_write_buffer_size(BIO *b, long size);
    long BIO_set_buffer_size(BIO *b, long size);
    long BIO_set_buffer_read_data(BIO *b, void *buf, long num);

# DESCRIPTION

BIO\_f\_buffer() returns the buffering BIO method.

Data written to a buffering BIO is buffered and periodically written
to the next BIO in the chain. Data read from a buffering BIO comes from
an internal buffer which is filled from the next BIO in the chain.
Both BIO\_gets() and BIO\_puts() are supported.

Calling BIO\_reset() on a buffering BIO clears any buffered data.

BIO\_get\_buffer\_num\_lines() returns the number of lines currently buffered.

BIO\_set\_read\_buffer\_size(), BIO\_set\_write\_buffer\_size() and BIO\_set\_buffer\_size()
set the read, write or both read and write buffer sizes to **size**. The initial
buffer size is DEFAULT\_BUFFER\_SIZE, currently 4096. Any attempt to reduce the
buffer size below DEFAULT\_BUFFER\_SIZE is ignored. Any buffered data is cleared
when the buffer is resized.

BIO\_set\_buffer\_read\_data() clears the read buffer and fills it with **num**
bytes of **buf**. If **num** is larger than the current buffer size the buffer
is expanded.

# NOTES

These functions, other than BIO\_f\_buffer(), are implemented as macros.

Buffering BIOs implement BIO\_gets() by using BIO\_read() operations on the
next BIO in the chain. By prepending a buffering BIO to a chain it is therefore
possible to provide BIO\_gets() functionality if the following BIOs do not
support it (for example SSL BIOs).

Data is only written to the next BIO in the chain when the write buffer fills
or when BIO\_flush() is called. It is therefore important to call BIO\_flush()
whenever any pending data should be written such as when removing a buffering
BIO using BIO\_pop(). BIO\_flush() may need to be retried if the ultimate
source/sink BIO is non blocking.

# RETURN VALUES

BIO\_f\_buffer() returns the buffering BIO method.

BIO\_get\_buffer\_num\_lines() returns the number of lines buffered (may be 0).

BIO\_set\_read\_buffer\_size(), BIO\_set\_write\_buffer\_size() and BIO\_set\_buffer\_size()
return 1 if the buffer was successfully resized or 0 for failure.

BIO\_set\_buffer\_read\_data() returns 1 if the data was set correctly or 0 if
there was an error.

# SEE ALSO

[BIO(3)](http://man.he.net/man3/BIO),
[BIO\_reset(3)](http://man.he.net/man3/BIO_reset),
[BIO\_flush(3)](http://man.he.net/man3/BIO_flush),
[BIO\_pop(3)](http://man.he.net/man3/BIO_pop),
[BIO\_ctrl(3)](http://man.he.net/man3/BIO_ctrl).

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
