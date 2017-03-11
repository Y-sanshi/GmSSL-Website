# NAME

EVP\_DigestSignInit, EVP\_DigestSignUpdate, EVP\_DigestSignFinal - EVP signing functions

# SYNOPSIS

    #include <openssl/evp.h>

    int EVP_DigestSignInit(EVP_MD_CTX *ctx, EVP_PKEY_CTX **pctx,
                           const EVP_MD *type, ENGINE *e, EVP_PKEY *pkey);
    int EVP_DigestSignUpdate(EVP_MD_CTX *ctx, const void *d, size_t cnt);
    int EVP_DigestSignFinal(EVP_MD_CTX *ctx, unsigned char *sig, size_t *siglen);

# DESCRIPTION

The EVP signature routines are a high level interface to digital signatures.

EVP\_DigestSignInit() sets up signing context **ctx** to use digest **type** from
ENGINE **impl** and private key **pkey**. **ctx** must be created with
EVP\_MD\_CTX\_new() before calling this function. If **pctx** is not NULL the
EVP\_PKEY\_CTX of the signing operation will be written to **\*pctx**: this can
be used to set alternative signing options.

EVP\_DigestSignUpdate() hashes **cnt** bytes of data at **d** into the
signature context **ctx**. This function can be called several times on the
same **ctx** to include additional data. This function is currently implemented
using a macro.

EVP\_DigestSignFinal() signs the data in **ctx** places the signature in **sig**.
If **sig** is **NULL** then the maximum size of the output buffer is written to
the **siglen** parameter. If **sig** is not **NULL** then before the call the
**siglen** parameter should contain the length of the **sig** buffer, if the
call is successful the signature is written to **sig** and the amount of data
written to **siglen**.

# RETURN VALUES

EVP\_DigestSignInit() EVP\_DigestSignUpdate() and EVP\_DigestSignaFinal() return
1 for success and 0 or a negative value for failure. In particular a return
value of -2 indicates the operation is not supported by the public key
algorithm.

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

The call to EVP\_DigestSignFinal() internally finalizes a copy of the digest
context. This means that calls to EVP\_DigestSignUpdate() and
EVP\_DigestSignFinal() can be called later to digest and sign additional data.

Since only a copy of the digest context is ever finalized the context must
be cleaned up after use by calling EVP\_MD\_CTX\_cleanup() or a memory leak
will occur.

The use of EVP\_PKEY\_size() with these functions is discouraged because some
signature operations may have a signature length which depends on the
parameters set. As a result EVP\_PKEY\_size() would have to return a value
which indicates the maximum possible signature for any set of parameters.

# SEE ALSO

[EVP\_DigestVerifyInit(3)](http://man.he.net/man3/EVP_DigestVerifyInit),
[EVP\_DigestInit(3)](http://man.he.net/man3/EVP_DigestInit), [err(3)](http://man.he.net/man3/err),
[evp(3)](http://man.he.net/man3/evp), [hmac(3)](http://man.he.net/man3/hmac), [md2(3)](http://man.he.net/man3/md2),
[md5(3)](http://man.he.net/man3/md5), [mdc2(3)](http://man.he.net/man3/mdc2), [ripemd(3)](http://man.he.net/man3/ripemd),
[sha(3)](http://man.he.net/man3/sha), [dgst(1)](http://man.he.net/man1/dgst)

# HISTORY

EVP\_DigestSignInit(), EVP\_DigestSignUpdate() and EVP\_DigestSignFinal()
were first added to OpenSSL 1.0.0.

# COPYRIGHT

Copyright 2006-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
