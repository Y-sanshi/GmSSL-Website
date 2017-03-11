# NAME

BIO\_ADDR, BIO\_ADDR\_new, BIO\_ADDR\_clear, BIO\_ADDR\_free, BIO\_ADDR\_rawmake,
BIO\_ADDR\_family, BIO\_ADDR\_rawaddress, BIO\_ADDR\_rawport,
BIO\_ADDR\_hostname\_string, BIO\_ADDR\_service\_string,
BIO\_ADDR\_path\_string - BIO\_ADDR routines

# SYNOPSIS

    #include <sys/types.h>
    #include <openssl/bio.h>

    typedef union bio_addr_st BIO_ADDR;

    BIO_ADDR *BIO_ADDR_new(void);
    void BIO_ADDR_free(BIO_ADDR *);
    void BIO_ADDR_clear(BIO_ADDR *ap);
    int BIO_ADDR_rawmake(BIO_ADDR *ap, int family,
                         const void *where, size_t wherelen, unsigned short port);
    int BIO_ADDR_family(const BIO_ADDR *ap);
    int BIO_ADDR_rawaddress(const BIO_ADDR *ap, void *p, size_t *l);
    unsigned short BIO_ADDR_rawport(const BIO_ADDR *ap);
    char *BIO_ADDR_hostname_string(const BIO_ADDR *ap, int numeric);
    char *BIO_ADDR_service_string(const BIO_ADDR *ap, int numeric);
    char *BIO_ADDR_path_string(const BIO_ADDR *ap);

# DESCRIPTION

The **BIO\_ADDR** type is a wrapper around all types of socket
addresses that OpenSSL deals with, currently transparently
supporting AF\_INET, AF\_INET6 and AF\_UNIX according to what's
available on the platform at hand.

BIO\_ADDR\_new() creates a new unfilled **BIO\_ADDR**, to be used
with routines that will fill it with information, such as
BIO\_accept\_ex().

BIO\_ADDR\_free() frees a **BIO\_ADDR** created with BIO\_ADDR\_new().

BIO\_ADDR\_clear() clears any data held within the provided **BIO\_ADDR** and sets
it back to an uninitialised state.

BIO\_ADDR\_rawmake() takes a protocol **family**, an byte array of
size **wherelen** with an address in network byte order pointed at
by **where** and a port number in network byte order in **port** (except
for the **AF\_UNIX** protocol family, where **port** is meaningless and
therefore ignored) and populates the given **BIO\_ADDR** with them.
In case this creates a **AF\_UNIX** **BIO\_ADDR**, **wherelen** is expected
to be the length of the path string (not including the terminating
NUL, such as the result of a call to strlen()).
_Read on about the addresses in ["RAW ADDRESSES"](#raw-addresses) below_.

BIO\_ADDR\_family() returns the protocol family of the given
**BIO\_ADDR**.  The possible non-error results are one of the
constants AF\_INET, AF\_INET6 and AF\_UNIX. It will also return AF\_UNSPEC if the
BIO\_ADDR has not been initialised.

BIO\_ADDR\_rawaddress() will write the raw address of the given
**BIO\_ADDR** in the area pointed at by **p** if **p** is non-NULL,
and will set **\*l** to be the amount of bytes the raw address
takes up if **l** is non-NULL.
A technique to only find out the size of the address is a call
with **p** set to **NULL**.  The raw address will be in network byte
order, most significant byte first.
In case this is a **AF\_UNIX** **BIO\_ADDR**, **l** gets the length of the
path string (not including the terminating NUL, such as the result of
a call to strlen()).
_Read on about the addresses in ["RAW ADDRESSES"](#raw-addresses) below_.

BIO\_ADDR\_rawport() returns the raw port of the given **BIO\_ADDR**.
The raw port will be in network byte order.

BIO\_ADDR\_hostname\_string() returns a character string with the
hostname of the given **BIO\_ADDR**.  If **numeric** is 1, the string
will contain the numerical form of the address.  This only works for
**BIO\_ADDR** of the protocol families AF\_INET and AF\_INET6.  The
returned string has been allocated on the heap and must be freed
with OPENSSL\_free().

BIO\_ADDR\_service\_string() returns a character string with the
service name of the port of the given **BIO\_ADDR**.  If **numeric**
is 1, the string will contain the port number.  This only works
for **BIO\_ADDR** of the protocol families AF\_INET and AF\_INET6.  The
returned string has been allocated on the heap and must be freed
with OPENSSL\_free().

BIO\_ADDR\_path\_string() returns a character string with the path
of the given **BIO\_ADDR**.  This only works for **BIO\_ADDR** of the
protocol family AF\_UNIX.  The returned string has been allocated
on the heap and must be freed with OPENSSL\_free().

# RAW ADDRESSES

Both BIO\_ADDR\_rawmake() and BIO\_ADDR\_rawaddress() take a pointer to a
network byte order address of a specific site.  Internally, those are
treated as a pointer to **struct in\_addr** (for **AF\_INET**), **struct
in6\_addr** (for **AF\_INET6**) or **char \*** (for **AF\_UNIX**), all
depending on the protocol family the address is for.

# RETURN VALUES

The string producing functions BIO\_ADDR\_hostname\_string(),
BIO\_ADDR\_service\_string() and BIO\_ADDR\_path\_string() will
return **NULL** on error and leave an error indication on the
OpenSSL error stack.

All other functions described here return 0 or **NULL** when the
information they should return isn't available.

# SEE ALSO

[BIO\_connect(3)](http://man.he.net/man3/BIO_connect), [BIO\_s\_connect(3)](http://man.he.net/man3/BIO_s_connect)

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
