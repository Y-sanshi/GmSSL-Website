# NAME

evp - high-level cryptographic functions

# SYNOPSIS

    #include <openssl/evp.h>

# DESCRIPTION

The EVP library provides a high-level interface to cryptographic
functions.

[**EVP\_Seal**_..._](http://man.he.net/man3/EVP_SealInit) and [**EVP\_Open**_..._](http://man.he.net/man3/EVP_OpenInit)
provide public key encryption and decryption to implement digital "envelopes".

The [**EVP\_DigestSign**_..._](http://man.he.net/man3/EVP_DigestSignInit) and
[**EVP\_DigestVerify**_..._](http://man.he.net/man3/EVP_DigestVerifyInit) functions implement
digital signatures and Message Authentication Codes (MACs). Also see the older
[**EVP\_Sign**_..._](http://man.he.net/man3/EVP_SignInit) and [**EVP\_Verify**_..._](http://man.he.net/man3/EVP_VerifyInit)
functions.

Symmetric encryption is available with the [**EVP\_Encrypt**_..._](http://man.he.net/man3/EVP_EncryptInit)
functions.  The [**EVP\_Digest**_..._](http://man.he.net/man3/EVP_DigestInit) functions provide message digests.

The **EVP\_PKEY**_..._ functions provide a high level interface to
asymmetric algorithms. To create a new EVP\_PKEY see
[EVP\_PKEY\_new(3)](http://man.he.net/man3/EVP_PKEY_new). EVP\_PKEYs can be associated
with a private key of a particular algorithm by using the functions
described on the [EVP\_PKEY\_set1\_RSA(3)](http://man.he.net/man3/EVP_PKEY_set1_RSA) page, or
new keys can be generated using [EVP\_PKEY\_keygen(3)](http://man.he.net/man3/EVP_PKEY_keygen).
EVP\_PKEYs can be compared using [EVP\_PKEY\_cmp(3)](http://man.he.net/man3/EVP_PKEY_cmp), or printed using
[EVP\_PKEY\_print\_private(3)](http://man.he.net/man3/EVP_PKEY_print_private).

The EVP\_PKEY functions support the full range of asymmetric algorithm operations:

- For key agreement see [EVP\_PKEY\_derive(3)](http://man.he.net/man3/EVP_PKEY_derive)
- For signing and verifying see [EVP\_PKEY\_sign(3)](http://man.he.net/man3/EVP_PKEY_sign),
[EVP\_PKEY\_verify(3)](http://man.he.net/man3/EVP_PKEY_verify) and [EVP\_PKEY\_verify\_recover(3)](http://man.he.net/man3/EVP_PKEY_verify_recover).
However, note that
these functions do not perform a digest of the data to be signed. Therefore
normally you would use the [EVP\_DigestSignInit(3)](http://man.he.net/man3/EVP_DigestSignInit)
functions for this purpose.
- For encryption and decryption see [EVP\_PKEY\_encrypt(3)](http://man.he.net/man3/EVP_PKEY_encrypt)
and [EVP\_PKEY\_decrypt(3)](http://man.he.net/man3/EVP_PKEY_decrypt) respectively. However, note that
these functions perform encryption and decryption only. As public key
encryption is an expensive operation, normally you would wrap
an encrypted message in a "digital envelope" using the [EVP\_SealInit(3)](http://man.he.net/man3/EVP_SealInit) and
[EVP\_OpenInit(3)](http://man.he.net/man3/EVP_OpenInit) functions.

The [EVP\_BytesToKey(3)](http://man.he.net/man3/EVP_BytesToKey) function provides some limited support for password
based encryption. Careful selection of the parameters will provide a PKCS#5 PBKDF1 compatible
implementation. However, new applications should not typically use this (preferring, for example,
PBKDF2 from PCKS#5).

The [**EVP\_Encode**_..._](http://man.he.net/man3/EVP_EncodeInit) and
[**EVP\_Decode**_..._](http://man.he.net/man3/EVP_EncodeInit) functions implement base 64 encoding
and decoding.

All the symmetric algorithms (ciphers), digests and asymmetric algorithms
(public key algorithms) can be replaced by [engine(3)](http://man.he.net/man3/engine) modules providing alternative
implementations. If ENGINE implementations of ciphers or digests are registered
as defaults, then the various EVP functions will automatically use those
implementations automatically in preference to built in software
implementations. For more information, consult the engine(3) man page.

Although low level algorithm specific functions exist for many algorithms
their use is discouraged. They cannot be used with an ENGINE and ENGINE
versions of new algorithms cannot be accessed using the low level functions.
Also makes code harder to adapt to new algorithms and some options are not
cleanly supported at the low level and some operations are more efficient
using the high level interface.

# SEE ALSO

[EVP\_DigestInit(3)](http://man.he.net/man3/EVP_DigestInit),
[EVP\_EncryptInit(3)](http://man.he.net/man3/EVP_EncryptInit),
[EVP\_OpenInit(3)](http://man.he.net/man3/EVP_OpenInit),
[EVP\_SealInit(3)](http://man.he.net/man3/EVP_SealInit),
[EVP\_DigestSignInit(3)](http://man.he.net/man3/EVP_DigestSignInit),
[EVP\_SignInit(3)](http://man.he.net/man3/EVP_SignInit),
[EVP\_VerifyInit(3)](http://man.he.net/man3/EVP_VerifyInit),
[EVP\_EncodeInit(3)](http://man.he.net/man3/EVP_EncodeInit),
[EVP\_PKEY\_new(3)](http://man.he.net/man3/EVP_PKEY_new),
[EVP\_PKEY\_set1\_RSA(3)](http://man.he.net/man3/EVP_PKEY_set1_RSA),
[EVP\_PKEY\_keygen(3)](http://man.he.net/man3/EVP_PKEY_keygen),
[EVP\_PKEY\_print\_private(3)](http://man.he.net/man3/EVP_PKEY_print_private),
[EVP\_PKEY\_decrypt(3)](http://man.he.net/man3/EVP_PKEY_decrypt),
[EVP\_PKEY\_encrypt(3)](http://man.he.net/man3/EVP_PKEY_encrypt),
[EVP\_PKEY\_sign(3)](http://man.he.net/man3/EVP_PKEY_sign),
[EVP\_PKEY\_verify(3)](http://man.he.net/man3/EVP_PKEY_verify),
[EVP\_PKEY\_verify\_recover(3)](http://man.he.net/man3/EVP_PKEY_verify_recover),
[EVP\_PKEY\_derive(3)](http://man.he.net/man3/EVP_PKEY_derive),
[EVP\_BytesToKey(3)](http://man.he.net/man3/EVP_BytesToKey),
[engine(3)](http://man.he.net/man3/engine)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
