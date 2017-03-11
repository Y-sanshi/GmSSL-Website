# NAME

EVP\_BytesToKey - password based encryption routine

# SYNOPSIS

    #include <openssl/evp.h>

    int EVP_BytesToKey(const EVP_CIPHER *type, const EVP_MD *md,
                       const unsigned char *salt,
                       const unsigned char *data, int datal, int count,
                       unsigned char *key, unsigned char *iv);

# DESCRIPTION

EVP\_BytesToKey() derives a key and IV from various parameters. **type** is
the cipher to derive the key and IV for. **md** is the message digest to use.
The **salt** parameter is used as a salt in the derivation: it should point to
an 8 byte buffer or NULL if no salt is used. **data** is a buffer containing
**datal** bytes which is used to derive the keying data. **count** is the
iteration count to use. The derived key and IV will be written to **key**
and **iv** respectively.

# NOTES

A typical application of this function is to derive keying material for an
encryption algorithm from a password in the **data** parameter.

Increasing the **count** parameter slows down the algorithm which makes it
harder for an attacker to perform a brute force attack using a large number
of candidate passwords.

If the total key and IV length is less than the digest length and
**MD5** is used then the derivation algorithm is compatible with PKCS#5 v1.5
otherwise a non standard extension is used to derive the extra data.

Newer applications should use a more modern algorithm such as PBKDF2 as
defined in PKCS#5v2.1 and provided by PKCS5\_PBKDF2\_HMAC.

# KEY DERIVATION ALGORITHM

The key and IV is derived by concatenating D\_1, D\_2, etc until
enough data is available for the key and IV. D\_i is defined as:

        D_i = HASH^count(D_(i-1) || data || salt)

where || denotes concatenation, D\_0 is empty, HASH is the digest
algorithm in use, HASH^1(data) is simply HASH(data), HASH^2(data)
is HASH(HASH(data)) and so on.

The initial bytes are used for the key and the subsequent bytes for
the IV.

# RETURN VALUES

If **data** is NULL, then EVP\_BytesToKey() returns the number of bytes
needed to store the derived key.
Otherwise, EVP\_BytesToKey() returns the size of the derived key in bytes,
or 0 on error.

# SEE ALSO

[evp(3)](http://man.he.net/man3/evp), [rand(3)](http://man.he.net/man3/rand),
[PKCS5\_PBKDF2\_HMAC(3)](http://man.he.net/man3/PKCS5_PBKDF2_HMAC),
[EVP\_EncryptInit(3)](http://man.he.net/man3/EVP_EncryptInit)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
