# NAME

OpenSSL\_add\_all\_algorithms, OpenSSL\_add\_all\_ciphers, OpenSSL\_add\_all\_digests, EVP\_cleanup -
add algorithms to internal table

# SYNOPSIS

    #include <openssl/evp.h>

Deprecated:

    # if OPENSSL_API_COMPAT < 0x10100000L
    void OpenSSL_add_all_algorithms(void);
    void OpenSSL_add_all_ciphers(void);
    void OpenSSL_add_all_digests(void);

    void EVP_cleanup(void)
   # endif

# DESCRIPTION

OpenSSL keeps an internal table of digest algorithms and ciphers. It uses
this table to lookup ciphers via functions such as EVP\_get\_cipher\_byname(). In
OpenSSL versions prior to 1.1.0 these functions initialised and de-initialised
this table. From OpenSSL 1.1.0 they are deprecated. No explicit initialisation
or de-initialisation is required. See [OPENSSL\_init\_crypto(3)](http://man.he.net/man3/OPENSSL_init_crypto) for further
information.

OpenSSL\_add\_all\_digests() adds all digest algorithms to the table.

OpenSSL\_add\_all\_algorithms() adds all algorithms to the table (digests and
ciphers).

OpenSSL\_add\_all\_ciphers() adds all encryption algorithms to the table including
password based encryption algorithms.

In versions prior to 1.1.0 EVP\_cleanup() removed all ciphers and digests from
the table. It no longer has any effect in OpenSSL 1.1.0.

# RETURN VALUES

None of the functions return a value.

# NOTES

A typical application will call OpenSSL\_add\_all\_algorithms() initially and
EVP\_cleanup() before exiting.

An application does not need to add algorithms to use them explicitly, for example
by EVP\_sha1(). It just needs to add them if it (or any of the functions it calls)
needs to lookup algorithms.

The cipher and digest lookup functions are used in many parts of the library. If
the table is not initialized several functions will misbehave and complain they
cannot find algorithms. This includes the PEM, PKCS#12, SSL and S/MIME libraries.
This is a common query in the OpenSSL mailing lists.

Calling OpenSSL\_add\_all\_algorithms() links in all algorithms: as a result a
statically linked executable can be quite large. If this is important it is possible
to just add the required ciphers and digests.

# BUGS

Although the functions do not return error codes it is possible for them to fail.
This will only happen as a result of a memory allocation failure so this is not
too much of a problem in practice.

# SEE ALSO

[evp(3)](http://man.he.net/man3/evp), [EVP\_DigestInit(3)](http://man.he.net/man3/EVP_DigestInit),
[EVP\_EncryptInit(3)](http://man.he.net/man3/EVP_EncryptInit)

# HISTORY

The OpenSSL\_add\_all\_algorithms(), OpenSSL\_add\_all\_ciphers(),
OpenSSL\_add\_all\_digests(), and EVP\_cleanup(), functions
were deprecated in OpenSSL 1.1.0 by OPENSSL\_init\_crypto().

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
