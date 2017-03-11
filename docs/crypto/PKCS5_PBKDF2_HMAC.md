# NAME

PKCS5\_PBKDF2\_HMAC, PKCS5\_PBKDF2\_HMAC\_SHA1 - password based derivation routines with salt and iteration count

# SYNOPSIS

    #include <openssl/evp.h>

    int PKCS5_PBKDF2_HMAC(const char *pass, int passlen,
                          const unsigned char *salt, int saltlen, int iter,
                          const EVP_MD *digest,
                          int keylen, unsigned char *out);

int PKCS5\_PBKDF2\_HMAC\_SHA1(const char \*pass, int passlen,
                           const unsigned char \*salt, int saltlen, int iter,
                           int keylen, unsigned char \*out);

# DESCRIPTION

PKCS5\_PBKDF2\_HMAC() derives a key from a password using a salt and iteration count
as specified in RFC 2898.

**pass** is the password used in the derivation of length **passlen**. **pass**
is an optional parameter and can be NULL. If **passlen** is -1, then the
function will calculate the length of **pass** using strlen().

**salt** is the salt used in the derivation of length **saltlen**. If the
**salt** is NULL, then **saltlen** must be 0. The function will not
attempt to calculate the length of the **salt** because it is not assumed to
be NULL terminated.

**iter** is the iteration count and its value should be greater than or
equal to 1. RFC 2898 suggests an iteration count of at least 1000. Any
**iter** less than 1 is treated as a single iteration.

**digest** is the message digest function used in the derivation. Values include
any of the EVP\_\* message digests. PKCS5\_PBKDF2\_HMAC\_SHA1() calls
PKCS5\_PBKDF2\_HMAC() with EVP\_sha1().

The derived key will be written to **out**. The size of the **out** buffer
is specified via **keylen**.

# NOTES

A typical application of this function is to derive keying material for an
encryption algorithm from a password in the **pass**, a salt in **salt**,
and an iteration count.

Increasing the **iter** parameter slows down the algorithm which makes it
harder for an attacker to perform a brute force attack using a large number
of candidate passwords.

# RETURN VALUES

PKCS5\_PBKDF2\_HMAC() and PBKCS5\_PBKDF2\_HMAC\_SHA1() return 1 on success or 0 on error.

# SEE ALSO

[evp(3)](http://man.he.net/man3/evp), [rand(3)](http://man.he.net/man3/rand),
[EVP\_BytesToKey(3)](http://man.he.net/man3/EVP_BytesToKey)

# COPYRIGHT

Copyright 2014-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
