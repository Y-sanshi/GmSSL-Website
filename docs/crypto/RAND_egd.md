# NAME

RAND\_egd, RAND\_egd\_bytes, RAND\_query\_egd\_bytes - query entropy gathering daemon

# SYNOPSIS

    #include <openssl/rand.h>

    int RAND_egd(const char *path);
    int RAND_egd_bytes(const char *path, int bytes);

    int RAND_query_egd_bytes(const char *path, unsigned char *buf, int bytes);

# DESCRIPTION

RAND\_egd() queries the entropy gathering daemon EGD on socket **path**.
It queries 255 bytes and uses [RAND\_add(3)](http://man.he.net/man3/RAND_add) to seed the
OpenSSL built-in PRNG. RAND\_egd(path) is a wrapper for
RAND\_egd\_bytes(path, 255);

RAND\_egd\_bytes() queries the entropy gathering daemon EGD on socket **path**.
It queries **bytes** bytes and uses [RAND\_add(3)](http://man.he.net/man3/RAND_add) to seed the
OpenSSL built-in PRNG.
This function is more flexible than RAND\_egd().
When only one secret key must
be generated, it is not necessary to request the full amount 255 bytes from
the EGD socket. This can be advantageous, since the amount of entropy
that can be retrieved from EGD over time is limited.

RAND\_query\_egd\_bytes() performs the actual query of the EGD daemon on socket
**path**. If **buf** is given, **bytes** bytes are queried and written into
**buf**. If **buf** is NULL, **bytes** bytes are queried and used to seed the
OpenSSL built-in PRNG using [RAND\_add(3)](http://man.he.net/man3/RAND_add).

# NOTES

On systems without /dev/\*random devices providing entropy from the kernel,
the EGD entropy gathering daemon can be used to collect entropy. It provides
a socket interface through which entropy can be gathered in chunks up to
255 bytes. Several chunks can be queried during one connection.

EGD is available from http://www.lothar.com/tech/crypto/ (`perl
Makefile.PL; make; make install` to install). It is run as **egd**
_path_, where _path_ is an absolute path designating a socket. When
RAND\_egd() is called with that path as an argument, it tries to read
random bytes that EGD has collected. RAND\_egd() retrieves entropy from the
daemon using the daemon's "non-blocking read" command which shall
be answered immediately by the daemon without waiting for additional
entropy to be collected. The write and read socket operations in the
communication are blocking.

Alternatively, the EGD-interface compatible daemon PRNGD can be used. It is
available from
http://prngd.sourceforge.net/ .
PRNGD does employ an internal PRNG itself and can therefore never run
out of entropy.

OpenSSL automatically queries EGD when entropy is requested via RAND\_bytes()
or the status is checked via RAND\_status() for the first time, if the socket
is located at /var/run/egd-pool, /dev/egd-pool or /etc/egd-pool.

# RETURN VALUE

RAND\_egd() and RAND\_egd\_bytes() return the number of bytes read from the
daemon on success, and -1 if the connection failed or the daemon did not
return enough data to fully seed the PRNG.

RAND\_query\_egd\_bytes() returns the number of bytes read from the daemon on
success, and -1 if the connection failed. The PRNG state is not considered.

# SEE ALSO

[rand(3)](http://man.he.net/man3/rand), [RAND\_add(3)](http://man.he.net/man3/RAND_add),
[RAND\_cleanup(3)](http://man.he.net/man3/RAND_cleanup)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
