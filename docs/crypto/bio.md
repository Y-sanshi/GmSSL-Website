# NAME

bio - Basic I/O abstraction

# SYNOPSIS

    #include <openssl/bio.h>

# DESCRIPTION

A BIO is an I/O abstraction, it hides many of the underlying I/O
details from an application. If an application uses a BIO for its
I/O it can transparently handle SSL connections, unencrypted network
connections and file I/O.

There are two type of BIO, a source/sink BIO and a filter BIO.

As its name implies a source/sink BIO is a source and/or sink of data,
examples include a socket BIO and a file BIO.

A filter BIO takes data from one BIO and passes it through to
another, or the application. The data may be left unmodified (for
example a message digest BIO) or translated (for example an
encryption BIO). The effect of a filter BIO may change according
to the I/O operation it is performing: for example an encryption
BIO will encrypt data if it is being written to and decrypt data
if it is being read from.

BIOs can be joined together to form a chain (a single BIO is a chain
with one component). A chain normally consist of one source/sink
BIO and one or more filter BIOs. Data read from or written to the
first BIO then traverses the chain to the end (normally a source/sink
BIO).

Some BIOs (such as memory BIOs) can be used immediately after calling
BIO\_new(). Others (such as file BIOs) need some additional initialization,
and frequently a utility function exists to create and initialize such BIOs.

If BIO\_free() is called on a BIO chain it will only free one BIO resulting
in a memory leak.

Calling BIO\_free\_all() a single BIO has the same effect as calling BIO\_free()
on it other than the discarded return value.

Normally the **type** argument is supplied by a function which returns a
pointer to a BIO\_METHOD. There is a naming convention for such functions:
a source/sink BIO is normally called BIO\_s\_\*() and a filter BIO
BIO\_f\_\*();

# EXAMPLE

Create a memory BIO:

    BIO *mem = BIO_new(BIO_s_mem());

# SEE ALSO

[BIO\_ctrl(3)](http://man.he.net/man3/BIO_ctrl),
[BIO\_f\_base64(3)](http://man.he.net/man3/BIO_f_base64), [BIO\_f\_buffer(3)](http://man.he.net/man3/BIO_f_buffer),
[BIO\_f\_cipher(3)](http://man.he.net/man3/BIO_f_cipher), [BIO\_f\_md(3)](http://man.he.net/man3/BIO_f_md),
[BIO\_f\_null(3)](http://man.he.net/man3/BIO_f_null), [BIO\_f\_ssl(3)](http://man.he.net/man3/BIO_f_ssl),
[BIO\_find\_type(3)](http://man.he.net/man3/BIO_find_type), [BIO\_new(3)](http://man.he.net/man3/BIO_new),
[BIO\_new\_bio\_pair(3)](http://man.he.net/man3/BIO_new_bio_pair),
[BIO\_push(3)](http://man.he.net/man3/BIO_push), [BIO\_read(3)](http://man.he.net/man3/BIO_read),
[BIO\_s\_accept(3)](http://man.he.net/man3/BIO_s_accept), [BIO\_s\_bio(3)](http://man.he.net/man3/BIO_s_bio),
[BIO\_s\_connect(3)](http://man.he.net/man3/BIO_s_connect), [BIO\_s\_fd(3)](http://man.he.net/man3/BIO_s_fd),
[BIO\_s\_file(3)](http://man.he.net/man3/BIO_s_file), [BIO\_s\_mem(3)](http://man.he.net/man3/BIO_s_mem),
[BIO\_s\_mem(3)](http://man.he.net/man3/BIO_s_mem),
[BIO\_s\_null(3)](http://man.he.net/man3/BIO_s_null), [BIO\_s\_socket(3)](http://man.he.net/man3/BIO_s_socket),
[BIO\_set\_callback(3)](http://man.he.net/man3/BIO_set_callback),
[BIO\_should\_retry(3)](http://man.he.net/man3/BIO_should_retry)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
