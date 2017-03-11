# NAME

EVP\_SealInit, EVP\_SealUpdate, EVP\_SealFinal - EVP envelope encryption

# SYNOPSIS

    #include <openssl/evp.h>

    int EVP_SealInit(EVP_CIPHER_CTX *ctx, const EVP_CIPHER *type,
                     unsigned char **ek, int *ekl, unsigned char *iv,
                     EVP_PKEY **pubk, int npubk);
    int EVP_SealUpdate(EVP_CIPHER_CTX *ctx, unsigned char *out,
            int *outl, unsigned char *in, int inl);
    int EVP_SealFinal(EVP_CIPHER_CTX *ctx, unsigned char *out,
            int *outl);

# DESCRIPTION

The EVP envelope routines are a high level interface to envelope
encryption. They generate a random key and IV (if required) then
"envelope" it by using public key encryption. Data can then be
encrypted using this key.

EVP\_SealInit() initializes a cipher context **ctx** for encryption
with cipher **type** using a random secret key and IV. **type** is normally
supplied by a function such as EVP\_aes\_256\_cbc(). The secret key is encrypted
using one or more public keys, this allows the same encrypted data to be
decrypted using any of the corresponding private keys. **ek** is an array of
buffers where the public key encrypted secret key will be written, each buffer
must contain enough room for the corresponding encrypted key: that is
**ek\[i\]** must have room for **EVP\_PKEY\_size(pubk\[i\])** bytes. The actual
size of each encrypted secret key is written to the array **ekl**. **pubk** is
an array of **npubk** public keys.

The **iv** parameter is a buffer where the generated IV is written to. It must
contain enough room for the corresponding cipher's IV, as determined by (for
example) EVP\_CIPHER\_iv\_length(type).

If the cipher does not require an IV then the **iv** parameter is ignored
and can be **NULL**.

EVP\_SealUpdate() and EVP\_SealFinal() have exactly the same properties
as the EVP\_EncryptUpdate() and EVP\_EncryptFinal() routines, as
documented on the [EVP\_EncryptInit(3)](http://man.he.net/man3/EVP_EncryptInit) manual
page.

# RETURN VALUES

EVP\_SealInit() returns 0 on error or **npubk** if successful.

EVP\_SealUpdate() and EVP\_SealFinal() return 1 for success and 0 for
failure.

# NOTES

Because a random secret key is generated the random number generator
must be seeded before calling EVP\_SealInit().

The public key must be RSA because it is the only OpenSSL public key
algorithm that supports key transport.

Envelope encryption is the usual method of using public key encryption
on large amounts of data, this is because public key encryption is slow
but symmetric encryption is fast. So symmetric encryption is used for
bulk encryption and the small random symmetric key used is transferred
using public key encryption.

It is possible to call EVP\_SealInit() twice in the same way as
EVP\_EncryptInit(). The first call should have **npubk** set to 0
and (after setting any cipher parameters) it should be called again
with **type** set to NULL.

# SEE ALSO

[evp(3)](http://man.he.net/man3/evp), [rand(3)](http://man.he.net/man3/rand),
[EVP\_EncryptInit(3)](http://man.he.net/man3/EVP_EncryptInit),
[EVP\_OpenInit(3)](http://man.he.net/man3/EVP_OpenInit)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
