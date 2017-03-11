# NAME

EVP\_VerifyInit\_ex,
EVP\_VerifyInit, EVP\_VerifyUpdate, EVP\_VerifyFinal
\- EVP signature verification functions

# SYNOPSIS

    #include <openssl/evp.h>

    int EVP_VerifyInit_ex(EVP_MD_CTX *ctx, const EVP_MD *type, ENGINE *impl);
    int EVP_VerifyUpdate(EVP_MD_CTX *ctx, const void *d, unsigned int cnt);
    int EVP_VerifyFinal(EVP_MD_CTX *ctx, unsigned char *sigbuf, unsigned int siglen, EVP_PKEY *pkey);

    int EVP_VerifyInit(EVP_MD_CTX *ctx, const EVP_MD *type);

# DESCRIPTION

The EVP signature verification routines are a high level interface to digital
signatures.

EVP\_VerifyInit\_ex() sets up verification context **ctx** to use digest
**type** from ENGINE **impl**. **ctx** must be created by calling
EVP\_MD\_CTX\_new() before calling this function.

EVP\_VerifyUpdate() hashes **cnt** bytes of data at **d** into the
verification context **ctx**. This function can be called several times on the
same **ctx** to include additional data.

EVP\_VerifyFinal() verifies the data in **ctx** using the public key **pkey**
and against the **siglen** bytes at **sigbuf**.

EVP\_VerifyInit() initializes verification context **ctx** to use the default
implementation of digest **type**.

# RETURN VALUES

EVP\_VerifyInit\_ex() and EVP\_VerifyUpdate() return 1 for success and 0 for
failure.

EVP\_VerifyFinal() returns 1 for a correct signature, 0 for failure and -1 if some
other error occurred.

The error codes can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# NOTES

The **EVP** interface to digital signatures should almost always be used in
preference to the low level interfaces. This is because the code then becomes
transparent to the algorithm used and much more flexible.

Due to the link between message digests and public key algorithms the correct
digest algorithm must be used with the correct public key type. A list of
algorithms and associated public key algorithms appears in
[EVP\_DigestInit(3)](http://man.he.net/man3/EVP_DigestInit).

The call to EVP\_VerifyFinal() internally finalizes a copy of the digest context.
This means that calls to EVP\_VerifyUpdate() and EVP\_VerifyFinal() can be called
later to digest and verify additional data.

Since only a copy of the digest context is ever finalized the context must
be cleaned up after use by calling EVP\_MD\_CTX\_cleanup() or a memory leak
will occur.

# BUGS

Older versions of this documentation wrongly stated that calls to
EVP\_VerifyUpdate() could not be made after calling EVP\_VerifyFinal().

Since the public key is passed in the call to EVP\_SignFinal() any error
relating to the private key (for example an unsuitable key and digest
combination) will not be indicated until after potentially large amounts of
data have been passed through EVP\_SignUpdate().

It is not possible to change the signing parameters using these function.

The previous two bugs are fixed in the newer EVP\_VerifyDigest\*() function.

# SEE ALSO

[evp(3)](http://man.he.net/man3/evp),
[EVP\_SignInit(3)](http://man.he.net/man3/EVP_SignInit),
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
