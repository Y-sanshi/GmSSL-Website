# NAME

RAND\_add, RAND\_seed, RAND\_status, RAND\_event, RAND\_screen - add
entropy to the PRNG

# SYNOPSIS

    #include <openssl/rand.h>

    void RAND_seed(const void *buf, int num);

    void RAND_add(const void *buf, int num, double entropy);

    int  RAND_status(void);

    #if OPENSSL_API_COMPAT < 0x10100000L
    int  RAND_event(UINT iMsg, WPARAM wParam, LPARAM lParam);
    void RAND_screen(void);
    #endif

# DESCRIPTION

RAND\_add() mixes the **num** bytes at **buf** into the PRNG state. Thus,
if the data at **buf** are unpredictable to an adversary, this
increases the uncertainty about the state and makes the PRNG output
less predictable. Suitable input comes from user interaction (random
key presses, mouse movements) and certain hardware events. The
**entropy** argument is (the lower bound of) an estimate of how much
randomness is contained in **buf**, measured in bytes. Details about
sources of randomness and how to estimate their entropy can be found
in the literature, e.g. RFC 1750.

RAND\_add() may be called with sensitive data such as user entered
passwords. The seed values cannot be recovered from the PRNG output.

OpenSSL makes sure that the PRNG state is unique for each thread. On
systems that provide `/dev/urandom`, the randomness device is used
to seed the PRNG transparently. However, on all other systems, the
application is responsible for seeding the PRNG by calling RAND\_add(),
[RAND\_egd(3)](http://man.he.net/man3/RAND_egd)
or [RAND\_load\_file(3)](http://man.he.net/man3/RAND_load_file).

RAND\_seed() is equivalent to RAND\_add() when **num == entropy**.

RAND\_event() and RAND\_screen() are deprecated and should not be called.

# RETURN VALUES

RAND\_status() returns 1 if the PRNG has been seeded
with enough data, 0 otherwise.

RAND\_event() calls RAND\_poll() and returns RAND\_status().

RAND\_screen calls RAND\_poll().

The other functions do not return values.

# HISTORY

RAND\_event() and RAND\_screen() are deprecated since OpenSSL
1.1.0.  Use the functions described above instead.

# SEE ALSO

[rand(3)](http://man.he.net/man3/rand), [RAND\_egd(3)](http://man.he.net/man3/RAND_egd),
[RAND\_load\_file(3)](http://man.he.net/man3/RAND_load_file), [RAND\_cleanup(3)](http://man.he.net/man3/RAND_cleanup)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
