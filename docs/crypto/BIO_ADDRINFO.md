# NAME

BIO\_ADDRINFO, BIO\_ADDRINFO\_next, BIO\_ADDRINFO\_free,
BIO\_ADDRINFO\_family, BIO\_ADDRINFO\_socktype, BIO\_ADDRINFO\_protocol,
BIO\_ADDRINFO\_address,
BIO\_lookup
\- BIO\_ADDRINFO type and routines

# SYNOPSIS

    #include <sys/types.h>
    #include <openssl/bio.h>

    typedef union bio_addrinfo_st BIO_ADDRINFO;

    enum BIO_lookup_type {
        BIO_LOOKUP_CLIENT, BIO_LOOKUP_SERVER
    };
    int BIO_lookup(const char *node, const char *service,
                   enum BIO_lookup_type lookup_type,
                   int family, int socktype, BIO_ADDRINFO **res);

    const BIO_ADDRINFO *BIO_ADDRINFO_next(const BIO_ADDRINFO *bai);
    int BIO_ADDRINFO_family(const BIO_ADDRINFO *bai);
    int BIO_ADDRINFO_socktype(const BIO_ADDRINFO *bai);
    int BIO_ADDRINFO_protocol(const BIO_ADDRINFO *bai);
    const BIO_ADDR *BIO_ADDRINFO_address(const BIO_ADDRINFO *bai);
    void BIO_ADDRINFO_free(BIO_ADDRINFO *bai);

# DESCRIPTION

The **BIO\_ADDRINFO** type is a wrapper for address information
types provided on your platform.

**BIO\_ADDRINFO** normally forms a chain of several that can be
picked at one by one.

BIO\_lookup() looks up a specified **host** and **service**, and
uses **lookup\_type** to determine what the default address should
be if **host** is **NULL**.  **family**, **socktype** are used to
determine what protocol family and protocol should be used for
the lookup.  **family** can be any of AF\_INET, AF\_INET6, AF\_UNIX and
AF\_UNSPEC, and **socktype** can be SOCK\_STREAM or SOCK\_DGRAM.
**res** points at a pointer to hold the start of a **BIO\_ADDRINFO**
chain.
For the family **AF\_UNIX**, BIO\_lookup() will ignore the **service**
parameter and expects the **node** parameter to hold the path to the
socket file.

BIO\_ADDRINFO\_family() returns the family of the given
**BIO\_ADDRINFO**.  The result will be one of the constants
AF\_INET, AF\_INET6 and AF\_UNIX.

BIO\_ADDRINFO\_socktype() returns the socket type of the given
**BIO\_ADDRINFO**.  The result will be one of the constants
SOCK\_STREAM and SOCK\_DGRAM.

BIO\_ADDRINFO\_protocol() returns the protocol id of the given
**BIO\_ADDRINFO**.  The result will be one of the constants
IPPROTO\_TCP and IPPROTO\_UDP.

BIO\_ADDRINFO\_address() returns the underlying **BIO\_ADDR**
of the given **BIO\_ADDRINFO**.

BIO\_ADDRINFO\_next() returns the next **BIO\_ADDRINFO** in the chain
from the given one.

BIO\_ADDRINFO\_free() frees the chain of **BIO\_ADDRINFO** starting
with the given one.

# RETURN VALUES

BIO\_lookup() returns 1 on success and 0 when an error occurred, and
will leave an error indication on the OpenSSL error stack in that case.

All other functions described here return 0 or **NULL** when the
information they should return isn't available.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
