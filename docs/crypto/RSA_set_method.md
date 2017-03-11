# NAME

RSA\_set\_default\_method, RSA\_get\_default\_method, RSA\_set\_method,
RSA\_get\_method, RSA\_PKCS1\_OpenSSL, RSA\_null\_method, RSA\_flags,
RSA\_new\_method - select RSA method

# SYNOPSIS

    #include <openssl/rsa.h>

    void RSA_set_default_method(const RSA_METHOD *meth);

    RSA_METHOD *RSA_get_default_method(void);

    int RSA_set_method(RSA *rsa, const RSA_METHOD *meth);

    RSA_METHOD *RSA_get_method(const RSA *rsa);

    RSA_METHOD *RSA_PKCS1_OpenSSL(void);

    RSA_METHOD *RSA_null_method(void);

    int RSA_flags(const RSA *rsa);

    RSA *RSA_new_method(ENGINE *engine);

# DESCRIPTION

An **RSA\_METHOD** specifies the functions that OpenSSL uses for RSA
operations. By modifying the method, alternative implementations such as
hardware accelerators may be used. IMPORTANT: See the NOTES section for
important information about how these RSA API functions are affected by the
use of **ENGINE** API calls.

Initially, the default RSA\_METHOD is the OpenSSL internal implementation,
as returned by RSA\_PKCS1\_OpenSSL().

RSA\_set\_default\_method() makes **meth** the default method for all RSA
structures created later. **NB**: This is true only whilst no ENGINE has
been set as a default for RSA, so this function is no longer recommended.

RSA\_get\_default\_method() returns a pointer to the current default
RSA\_METHOD. However, the meaningfulness of this result is dependent on
whether the ENGINE API is being used, so this function is no longer
recommended.

RSA\_set\_method() selects **meth** to perform all operations using the key
**rsa**. This will replace the RSA\_METHOD used by the RSA key and if the
previous method was supplied by an ENGINE, the handle to that ENGINE will
be released during the change. It is possible to have RSA keys that only
work with certain RSA\_METHOD implementations (eg. from an ENGINE module
that supports embedded hardware-protected keys), and in such cases
attempting to change the RSA\_METHOD for the key can have unexpected
results.

RSA\_get\_method() returns a pointer to the RSA\_METHOD being used by **rsa**.
This method may or may not be supplied by an ENGINE implementation, but if
it is, the return value can only be guaranteed to be valid as long as the
RSA key itself is valid and does not have its implementation changed by
RSA\_set\_method().

RSA\_flags() returns the **flags** that are set for **rsa**'s current
RSA\_METHOD. See the BUGS section.

RSA\_new\_method() allocates and initializes an RSA structure so that
**engine** will be used for the RSA operations. If **engine** is NULL, the
default ENGINE for RSA operations is used, and if no default ENGINE is set,
the RSA\_METHOD controlled by RSA\_set\_default\_method() is used.

RSA\_flags() returns the **flags** that are set for **rsa**'s current method.

RSA\_new\_method() allocates and initializes an **RSA** structure so that
**method** will be used for the RSA operations. If **method** is **NULL**,
the default method is used.

# THE RSA\_METHOD STRUCTURE

    typedef struct rsa_meth_st
    {
        /* name of the implementation */
           const char *name;

        /* encrypt */
           int (*rsa_pub_enc)(int flen, unsigned char *from,
             unsigned char *to, RSA *rsa, int padding);

        /* verify arbitrary data */
           int (*rsa_pub_dec)(int flen, unsigned char *from,
             unsigned char *to, RSA *rsa, int padding);

        /* sign arbitrary data */
           int (*rsa_priv_enc)(int flen, unsigned char *from,
             unsigned char *to, RSA *rsa, int padding);

        /* decrypt */
           int (*rsa_priv_dec)(int flen, unsigned char *from,
             unsigned char *to, RSA *rsa, int padding);

        /* compute r0 = r0 ^ I mod rsa->n (May be NULL for some
                                           implementations) */
           int (*rsa_mod_exp)(BIGNUM *r0, BIGNUM *I, RSA *rsa);

        /* compute r = a ^ p mod m (May be NULL for some implementations) */
           int (*bn_mod_exp)(BIGNUM *r, BIGNUM *a, const BIGNUM *p,
             const BIGNUM *m, BN_CTX *ctx, BN_MONT_CTX *m_ctx);

        /* called at RSA_new */
           int (*init)(RSA *rsa);

        /* called at RSA_free */
           int (*finish)(RSA *rsa);

        /* RSA_FLAG_EXT_PKEY        - rsa_mod_exp is called for private key
         *                            operations, even if p,q,dmp1,dmq1,iqmp
         *                            are NULL
         * RSA_METHOD_FLAG_NO_CHECK - don't check pub/private match
         */
           int flags;

           char *app_data; /* ?? */

           int (*rsa_sign)(int type,
                   const unsigned char *m, unsigned int m_length,
                   unsigned char *sigret, unsigned int *siglen, const RSA *rsa);
           int (*rsa_verify)(int dtype,
                   const unsigned char *m, unsigned int m_length,
                   const unsigned char *sigbuf, unsigned int siglen,
                                                                   const RSA *rsa);
        /* keygen. If NULL builtin RSA key generation will be used */
           int (*rsa_keygen)(RSA *rsa, int bits, BIGNUM *e, BN_GENCB *cb);

    } RSA_METHOD;

# RETURN VALUES

RSA\_PKCS1\_OpenSSL(), RSA\_PKCS1\_null\_method(), RSA\_get\_default\_method()
and RSA\_get\_method() return pointers to the respective RSA\_METHODs.

RSA\_set\_default\_method() returns no value.

RSA\_set\_method() returns a pointer to the old RSA\_METHOD implementation
that was replaced. However, this return value should probably be ignored
because if it was supplied by an ENGINE, the pointer could be invalidated
at any time if the ENGINE is unloaded (in fact it could be unloaded as a
result of the RSA\_set\_method() function releasing its handle to the
ENGINE). For this reason, the return type may be replaced with a **void**
declaration in a future release.

RSA\_new\_method() returns NULL and sets an error code that can be obtained
by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error) if the allocation fails. Otherwise
it returns a pointer to the newly allocated structure.

# BUGS

The behaviour of RSA\_flags() is a mis-feature that is left as-is for now
to avoid creating compatibility problems. RSA functionality, such as the
encryption functions, are controlled by the **flags** value in the RSA key
itself, not by the **flags** value in the RSA\_METHOD attached to the RSA key
(which is what this function returns). If the flags element of an RSA key
is changed, the changes will be honoured by RSA functionality but will not
be reflected in the return value of the RSA\_flags() function - in effect
RSA\_flags() behaves more like an RSA\_default\_flags() function (which does
not currently exist).

# SEE ALSO

[RSA\_new(3)](http://man.he.net/man3/RSA_new)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
