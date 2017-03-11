# NAME

EVP\_PKEY\_size,
EVP\_SignInit, EVP\_SignInit\_ex, EVP\_SignUpdate, EVP\_SignFinal - EVP signing
functions

# SYNOPSIS

    #include <openssl/evp.h>

    int EVP_SignInit_ex(EVP_MD_CTX *ctx, const EVP_MD *type, ENGINE *impl);
    int EVP_SignUpdate(EVP_MD_CTX *ctx, const void *d, unsigned int cnt);
    int EVP_SignFinal(EVP_MD_CTX *ctx, unsigned char *sig, unsigned int *s, EVP_PKEY *pkey);

    void EVP_SignInit(EVP_MD_CTX *ctx, const EVP_MD *type);

    int EVP_PKEY_size(EVP_PKEY *pkey);

# DESCRIPTION

The EVP signature routines are a high level interface to digital
signatures.

EVP\_SignInit\_ex() sets up signing context **ctx** to use digest
**type** from ENGINE **impl**. **ctx** must be created with
EVP\_MD\_CTX\_new() before calling this function.

EVP\_SignUpdate() hashes **cnt** bytes of data at **d** into the
signature context **ctx**. This function can be called several times on the
same **ctx** to include additional data.

EVP\_SignFinal() signs the data in **ctx** using the private key **pkey** and
places the signature in **sig**. **sig** must be at least EVP\_PKEY\_size(pkey)
bytes in size. **s** is an OUT parameter, and not used as an IN parameter.
The number of bytes of data written (i.e. the length of the signature)
will be written to the integer at **s**, at most EVP\_PKEY\_size(pkey) bytes
will be written.

EVP\_SignInit() initializes a signing context **ctx** to use the default
implementation of digest **type**.

EVP\_PKEY\_size() returns the maximum size of a signature in bytes. The actual
signature returned by EVP\_SignFinal() may be smaller.

# RETURN VALUES

EVP\_SignInit\_ex(), EVP\_SignUpdate() and EVP\_SignFinal() return 1
for success and 0 for failure.

EVP\_PKEY\_size() returns the maximum size of a signature in bytes.

The error codes can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# NOTES

The **EVP** interface to digital signatures should almost always be used in
preference to the low level interfaces. This is because the code then becomes
transparent to the algorithm used and much more flexible.

Due to the link between message digests and public key algorithms the correct
digest algorithm must be used with the correct public key type. A list of
algorithms and associated public key algorithms appears in
[EVP\_DigestInit(3)](http://man.he.net/man3/EVP_DigestInit).

When signing with DSA private keys the random number generator must be seeded
or the operation will fail. The random number generator does not need to be
seeded for RSA signatures.

The call to EVP\_SignFinal() internally finalizes a copy of the digest context.
This means that calls to EVP\_SignUpdate() and EVP\_SignFinal() can be called
later to digest and sign additional data.

Since only a copy of the digest context is ever finalized the context must
be cleaned up after use by calling EVP\_MD\_CTX\_cleanup() or a memory leak
will occur.

# BUGS

Older versions of this documentation wrongly stated that calls to
EVP\_SignUpdate() could not be made after calling EVP\_SignFinal().

Since the private key is passed in the call to EVP\_SignFinal() any error
relating to the private key (for example an unsuitable key and digest
combination) will not be indicated until after potentially large amounts of
data have been passed through EVP\_SignUpdate().

It is not possible to change the signing parameters using these function.

The previous two bugs are fixed in the newer EVP\_SignDigest\*() function.

# SEE ALSO

[EVP\_VerifyInit(3)](http://man.he.net/man3/EVP_VerifyInit),
[EVP\_DigestInit(3)](http://man.he.net/man3/EVP_DigestInit), [err(3)](http://man.he.net/man3/err),
[evp(3)](http://man.he.net/man3/evp), [hmac(3)](http://man.he.net/man3/hmac), [md2(3)](http://man.he.net/man3/md2),
[md5(3)](http://man.he.net/man3/md5), [mdc2(3)](http://man.he.net/man3/mdc2), [ripemd(3)](http://man.he.net/man3/ripemd),
[sha(3)](http://man.he.net/man3/sha), [dgst(1)](http://man.he.net/man1/dgst)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
