# NAME

ECDSA\_SIG\_get0, ECDSA\_SIG\_set0,
ECDSA\_SIG\_new, ECDSA\_SIG\_free, i2d\_ECDSA\_SIG, d2i\_ECDSA\_SIG, ECDSA\_size,
ECDSA\_sign, ECDSA\_do\_sign, ECDSA\_verify, ECDSA\_do\_verify, ECDSA\_sign\_setup,
ECDSA\_sign\_ex, ECDSA\_do\_sign\_ex - low level elliptic curve digital signature
algorithm (ECDSA) functions

# SYNOPSIS

    #include <openssl/ecdsa.h>

    ECDSA_SIG *ECDSA_SIG_new(void);
    void ECDSA_SIG_free(ECDSA_SIG *sig);
    void ECDSA_SIG_get0(const ECDSA_SIG *sig, const BIGNUM **pr, const BIGNUM **ps);
    int ECDSA_SIG_set0(ECDSA_SIG *sig, BIGNUM *r, BIGNUM *s);
    int i2d_ECDSA_SIG(const ECDSA_SIG *sig, unsigned char **pp);
    ECDSA_SIG *d2i_ECDSA_SIG(ECDSA_SIG **sig, const unsigned char **pp, long len);
    int ECDSA_size(const EC_KEY *eckey);

    int ECDSA_sign(int type, const unsigned char *dgst, int dgstlen,
                   unsigned char *sig, unsigned int *siglen, EC_KEY *eckey);
    ECDSA_SIG *ECDSA_do_sign(const unsigned char *dgst, int dgst_len,
                             EC_KEY *eckey);

    int ECDSA_verify(int type, const unsigned char *dgst, int dgstlen,
                     const unsigned char *sig, int siglen, EC_KEY *eckey);
    int ECDSA_do_verify(const unsigned char *dgst, int dgst_len,
                        const ECDSA_SIG *sig, EC_KEY* eckey);

    ECDSA_SIG *ECDSA_do_sign_ex(const unsigned char *dgst, int dgstlen,
                                const BIGNUM *kinv, const BIGNUM *rp,
                                EC_KEY *eckey);
    int ECDSA_sign_setup(EC_KEY *eckey, BN_CTX *ctx, BIGNUM **kinv, BIGNUM **rp);
    int ECDSA_sign_ex(int type, const unsigned char *dgst, int dgstlen,
                      unsigned char *sig, unsigned int *siglen,
                      const BIGNUM *kinv, const BIGNUM *rp, EC_KEY *eckey);

# DESCRIPTION

Note: these functions provide a low level interface to ECDSA. Most
applications should use the higher level **EVP** interface such as
[EVP\_DigestSignInit(3)](http://man.he.net/man3/EVP_DigestSignInit) or [EVP\_DigestVerifyInit(3)](http://man.he.net/man3/EVP_DigestVerifyInit) instead.

**ECDSA\_SIG** is an opaque structure consisting of two BIGNUMs for the
**r** and **s** value of an ECDSA signature (see X9.62 or FIPS 186-2).

ECDSA\_SIG\_new() allocates an empty **ECDSA\_SIG** structure. Note: before
OpenSSL 1.1.0 the: the **r** and **s** components were initialised.

ECDSA\_SIG\_free() frees the **ECDSA\_SIG** structure **sig**.

ECDSA\_SIG\_get0() returns internal pointers the **r** and **s** values contained
in **sig**.

The **r** and **s** values can be set by calling ECDSA\_SIG\_set0() and passing the
new values for **r** and **s** as parameters to the function. Calling this
function transfers the memory management of the values to the ECDSA\_SIG object,
and therefore the values that have been passed in should not be freed directly
after this function has been called.

i2d\_ECDSA\_SIG() creates the DER encoding of the ECDSA signature **sig** and
writes the encoded signature to **\*pp** (note: if **pp** is NULL i2d\_ECDSA\_SIG()
returns the expected length in bytes of the DER encoded signature).
i2d\_ECDSA\_SIG() returns the length of the DER encoded signature (or 0 on
error).

d2i\_ECDSA\_SIG() decodes a DER encoded ECDSA signature and returns the decoded
signature in a newly allocated **ECDSA\_SIG** structure. **\*sig** points to the
buffer containing the DER encoded signature of size **len**.

ECDSA\_size() returns the maximum length of a DER encoded ECDSA signature
created with the private EC key **eckey**.

ECDSA\_sign() computes a digital signature of the **dgstlen** bytes hash value
**dgst** using the private EC key **eckey**. The DER encoded signatures is
stored in **sig** and its length is returned in **sig\_len**. Note: **sig** must
point to ECDSA\_size(eckey) bytes of memory. The parameter **type** is currently
ignored. ECDSA\_sign() is wrapper function for ECDSA\_sign\_ex() with **kinv**
and **rp** set to NULL.

ECDSA\_do\_sign() is similar to ECDSA\_sign() except the signature is returned
as a newly allocated **ECDSA\_SIG** structure (or NULL on error). ECDSA\_do\_sign()
is a wrapper function for ECDSA\_do\_sign\_ex() with **kinv** and **rp** set to
NULL.

ECDSA\_verify() verifies that the signature in **sig** of size **siglen** is a
valid ECDSA signature of the hash value **dgst** of size **dgstlen** using the
public key **eckey**.  The parameter **type** is ignored.

ECDSA\_do\_verify() is similar to ECDSA\_verify() except the signature is
presented in the form of a pointer to an **ECDSA\_SIG** structure.

The remaining functions utilise the internal **kinv** and **r** values used
during signature computation. Most applications will never need to call these
and some external ECDSA ENGINE implementations may not support them at all if
either **kinv** or **r** is not **NULL**.

ECDSA\_sign\_setup() may be used to precompute parts of the signing operation.
**eckey** is the private EC key and **ctx** is a pointer to **BN\_CTX** structure
(or NULL). The precomputed values or returned in **kinv** and **rp** and can be
used in a later call to ECDSA\_sign\_ex() or ECDSA\_do\_sign\_ex().

ECDSA\_sign\_ex() computes a digital signature of the **dgstlen** bytes hash value
**dgst** using the private EC key **eckey** and the optional pre-computed values
**kinv** and **rp**. The DER encoded signature is stored in **sig** and its
length is returned in **sig\_len**. Note: **sig** must point to ECDSA\_size(eckey)
bytes of memory. The parameter **type** is ignored.

ECDSA\_do\_sign\_ex() is similar to ECDSA\_sign\_ex() except the signature is
returned as a newly allocated **ECDSA\_SIG** structure (or NULL on error).

# RETURN VALUES

ECDSA\_SIG\_set0() returns 1 on success or 0 on failure.

ECDSA\_size() returns the maximum length signature or 0 on error.

ECDSA\_sign(), ECDSA\_sign\_ex() and ECDSA\_sign\_setup() return 1 if successful
or 0 on error.

ECDSA\_do\_sign() and ECDSA\_do\_sign\_ex() return a pointer to an allocated
**ECDSA\_SIG** structure or NULL on error.

ECDSA\_verify() and ECDSA\_do\_verify() return 1 for a valid
signature, 0 for an invalid signature and -1 on error.
The error codes can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# EXAMPLES

Creating an ECDSA signature of a given SHA-256 hash value using the
named curve prime256v1 (aka P-256).

First step: create an EC\_KEY object (note: this part is **not** ECDSA
specific)

    int        ret;
    ECDSA_SIG *sig;
    EC_KEY    *eckey;
    eckey = EC_KEY_new_by_curve_name(NID_X9_62_prime256v1);
    if (eckey == NULL) {
       /* error */
    }
    if (EC_KEY_generate_key(eckey) == 0) {
       /* error */
    }

Second step: compute the ECDSA signature of a SHA-256 hash value
using ECDSA\_do\_sign():

    sig = ECDSA_do_sign(digest, 32, eckey);
    if (sig == NULL) {
       /* error */
    }

or using ECDSA\_sign():

    unsigned char *buffer, *pp;
    int            buf_len;
    buf_len = ECDSA_size(eckey);
    buffer  = OPENSSL_malloc(buf_len);
    pp = buffer;
    if (ECDSA_sign(0, dgst, dgstlen, pp, &buf_len, eckey) == 0) {
       /* error */
    }

Third step: verify the created ECDSA signature using ECDSA\_do\_verify():

    ret = ECDSA_do_verify(digest, 32, sig, eckey);

or using ECDSA\_verify():

    ret = ECDSA_verify(0, digest, 32, buffer, buf_len, eckey);

and finally evaluate the return value:

    if (ret == 1) {
       /* signature ok */
    } else if (ret == 0) {
       /* incorrect signature */
    } else {
       /* error */
    }

# CONFORMING TO

ANSI X9.62, US Federal Information Processing Standard FIPS 186-2
(Digital Signature Standard, DSS)

# SEE ALSO

[DSA\_new(3)](http://man.he.net/man3/DSA_new),
[EVP\_DigestSignInit(3)](http://man.he.net/man3/EVP_DigestSignInit),
[EVP\_DigestVerifyInit(3)](http://man.he.net/man3/EVP_DigestVerifyInit)

# COPYRIGHT

Copyright 2004-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
