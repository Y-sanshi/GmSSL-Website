# NAME

EVP\_DigestVerifyInit, EVP\_DigestVerifyUpdate, EVP\_DigestVerifyFinal - EVP signature verification functions

# SYNOPSIS

    #include <openssl/evp.h>

    int EVP_DigestVerifyInit(EVP_MD_CTX *ctx, EVP_PKEY_CTX **pctx,
                           const EVP_MD *type, ENGINE *e, EVP_PKEY *pkey);
    int EVP_DigestVerifyUpdate(EVP_MD_CTX *ctx, const void *d, size_t cnt);
    int EVP_DigestVerifyFinal(EVP_MD_CTX *ctx, const unsigned char *sig, size_t siglen);

# DESCRIPTION

The EVP signature routines are a high level interface to digital signatures.

EVP\_DigestVerifyInit() sets up verification context **ctx** to use digest
**type** from ENGINE **impl** and public key **pkey**. **ctx** must be created
with EVP\_MD\_CTX\_new() before calling this function. If **pctx** is not NULL the
EVP\_PKEY\_CTX of the verification operation will be written to **\*pctx**: this
can be used to set alternative verification options.

EVP\_DigestVerifyUpdate() hashes **cnt** bytes of data at **d** into the
verification context **ctx**. This function can be called several times on the
same **ctx** to include additional data. This function is currently implemented
using a macro.

EVP\_DigestVerifyFinal() verifies the data in **ctx** against the signature in
**sig** of length **siglen**.

# RETURN VALUES

EVP\_DigestVerifyInit() and EVP\_DigestVerifyUpdate() return 1 for success and 0
for failure.

EVP\_DigestVerifyFinal() returns 1 for success; any other value indicates
failure.  A return value of zero indicates that the signature did not verify
successfully (that is, tbs did not match the original data or the signature had
an invalid form), while other values indicate a more serious error (and
sometimes also indicate an invalid signature form).

The error codes can be obtained from [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# NOTES

The **EVP** interface to digital signatures should almost always be used in
preference to the low level interfaces. This is because the code then becomes
transparent to the algorithm used and much more flexible.

In previous versions of OpenSSL there was a link between message digest types
and public key algorithms. This meant that "clone" digests such as EVP\_dss1()
needed to be used to sign using SHA1 and DSA. This is no longer necessary and
the use of clone digest is now discouraged.

For some key types and parameters the random number generator must be seeded
or the operation will fail.

The call to EVP\_DigestVerifyFinal() internally finalizes a copy of the digest
context. This means that EVP\_VerifyUpdate() and EVP\_VerifyFinal() can
be called later to digest and verify additional data.

Since only a copy of the digest context is ever finalized the context must
be cleaned up after use by calling EVP\_MD\_CTX\_cleanup() or a memory leak
will occur.

# SEE ALSO

[EVP\_DigestSignInit(3)](http://man.he.net/man3/EVP_DigestSignInit),
[EVP\_DigestInit(3)](http://man.he.net/man3/EVP_DigestInit), [err(3)](http://man.he.net/man3/err),
[evp(3)](http://man.he.net/man3/evp), [hmac(3)](http://man.he.net/man3/hmac), [md2(3)](http://man.he.net/man3/md2),
[md5(3)](http://man.he.net/man3/md5), [mdc2(3)](http://man.he.net/man3/mdc2), [ripemd(3)](http://man.he.net/man3/ripemd),
[sha(3)](http://man.he.net/man3/sha), [dgst(1)](http://man.he.net/man1/dgst)

# HISTORY

EVP\_DigestVerifyInit(), EVP\_DigestVerifyUpdate() and EVP\_DigestVerifyFinal()
were first added to OpenSSL 1.0.0.

# COPYRIGHT

Copyright 2006-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
