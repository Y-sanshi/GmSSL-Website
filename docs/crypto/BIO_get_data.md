# NAME

BIO\_set\_data, BIO\_get\_data, BIO\_set\_init, BIO\_get\_init, BIO\_set\_shutdown,
BIO\_get\_shutdown - functions for managing BIO state information

# SYNOPSIS

    #include <openssl/bio.h>

    void BIO_set_data(BIO *a, void *ptr);
    void *BIO_get_data(BIO *a);
    void BIO_set_init(BIO *a, int init);
    int BIO_get_init(BIO *a);
    void BIO_set_shutdown(BIO *a, int shut);
    int BIO_get_shutdown(BIO *a);

# DESCRIPTION

These functions are mainly useful when implementing a custom BIO.

The BIO\_set\_data() function associates the custom data pointed to by **ptr** with
the BIO. This data can subsequently be retrieved via a call to BIO\_get\_data().
This can be used by custom BIOs for storing implementation specific information.

The BIO\_set\_init() function sets the value of the BIO's "init" flag to indicate
whether initialisation has been completed for this BIO or not. A non-zero value
indicates that initialisation is complete, whilst zero indicates that it is not.
Often initialisation will complete during initial construction of the BIO. For
some BIOs however, initialisation may not complete until after additional steps
have occurred (for example through calling custom ctrls). The BIO\_get\_init()
function returns the value of the "init" flag.

The BIO\_set\_shutdown() and BIO\_get\_shutdown() functions set and get the state of
this BIO's shutdown (i.e. BIO\_CLOSE) flag. If set then the underlying resource
is also closed when the BIO is freed.

# RETURN VALUES

BIO\_get\_data() returns a pointer to the implementation specific custom data
associated with this BIO, or NULL if none has been set.

BIO\_get\_init() returns the state of the BIO's init flag.

BIO\_get\_shutdown() returns the stat of the BIO's shutdown (i.e. BIO\_CLOSE) flag.

# SEE ALSO

[bio](https://metacpan.org/pod/bio), [BIO\_meth\_new](https://metacpan.org/pod/BIO_meth_new)

# HISTORY

The functions described here were added in OpenSSL version 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
