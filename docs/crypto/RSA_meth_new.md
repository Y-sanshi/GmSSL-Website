# NAME

RSA\_meth\_get0\_app\_data, RSA\_meth\_set0\_app\_data,
RSA\_meth\_new, RSA\_meth\_free, RSA\_meth\_dup, RSA\_meth\_get0\_name,
RSA\_meth\_set1\_name, RSA\_meth\_get\_flags, RSA\_meth\_set\_flags,
RSA\_meth\_get\_pub\_enc,
RSA\_meth\_set\_pub\_enc, RSA\_meth\_get\_pub\_dec, RSA\_meth\_set\_pub\_dec,
RSA\_meth\_get\_priv\_enc, RSA\_meth\_set\_priv\_enc, RSA\_meth\_get\_priv\_dec,
RSA\_meth\_set\_priv\_dec, RSA\_meth\_get\_mod\_exp, RSA\_meth\_set\_mod\_exp,
RSA\_meth\_get\_bn\_mod\_exp, RSA\_meth\_set\_bn\_mod\_exp, RSA\_meth\_get\_init,
RSA\_meth\_set\_init, RSA\_meth\_get\_finish, RSA\_meth\_set\_finish,
RSA\_meth\_get\_sign, RSA\_meth\_set\_sign, RSA\_meth\_get\_verify,
RSA\_meth\_set\_verify, RSA\_meth\_get\_keygen, RSA\_meth\_set\_keygen
\- Routines to build up RSA methods

# SYNOPSIS

    #include <openssl/rsa.h>

    RSA_METHOD *RSA_meth_new(const char *name, int flags);
    void RSA_meth_free(RSA_METHOD *meth);
    RSA_METHOD *RSA_meth_dup(const RSA_METHOD *meth);
    const char *RSA_meth_get0_name(const RSA_METHOD *meth);
    int RSA_meth_set1_name(RSA_METHOD *meth, const char *name);
    int RSA_meth_get_flags(RSA_METHOD *meth);
    int RSA_meth_set_flags(RSA_METHOD *meth, int flags);
    void *RSA_meth_get0_app_data(const RSA_METHOD *meth);
    int RSA_meth_set0_app_data(RSA_METHOD *meth, void *app_data);
    int (*RSA_meth_get_pub_enc(const RSA_METHOD *meth))
        (int flen, const unsigned char *from,
         unsigned char *to, RSA *rsa, int padding);
    int RSA_meth_set_pub_enc(RSA_METHOD *rsa,
                             int (*pub_enc) (int flen, const unsigned char *from,
                                             unsigned char *to, RSA *rsa,
                                             int padding));
    int (*RSA_meth_get_pub_dec(const RSA_METHOD *meth))
        (int flen, const unsigned char *from,
         unsigned char *to, RSA *rsa, int padding);
    int RSA_meth_set_pub_dec(RSA_METHOD *rsa,
                             int (*pub_dec) (int flen, const unsigned char *from,
                                             unsigned char *to, RSA *rsa,
                                             int padding));
    int (*RSA_meth_get_priv_enc(const RSA_METHOD *meth))
        (int flen, const unsigned char *from,
         unsigned char *to, RSA *rsa, int padding);
    int RSA_meth_set_priv_enc(RSA_METHOD *rsa,
                              int (*priv_enc) (int flen, const unsigned char *from,
                                               unsigned char *to, RSA *rsa,
                                               int padding));
    int (*RSA_meth_get_priv_dec(const RSA_METHOD *meth))
        (int flen, const unsigned char *from,
         unsigned char *to, RSA *rsa, int padding);
    int RSA_meth_set_priv_dec(RSA_METHOD *rsa,
                              int (*priv_dec) (int flen, const unsigned char *from,
                                               unsigned char *to, RSA *rsa,
                                               int padding));
        /* Can be null */
    int (*RSA_meth_get_mod_exp(const RSA_METHOD *meth))
        (BIGNUM *r0, const BIGNUM *I, RSA *rsa, BN_CTX *ctx);
    int RSA_meth_set_mod_exp(RSA_METHOD *rsa,
                             int (*mod_exp) (BIGNUM *r0, const BIGNUM *I, RSA *rsa,
                                             BN_CTX *ctx));
        /* Can be null */
    int (*RSA_meth_get_bn_mod_exp(const RSA_METHOD *meth))
        (BIGNUM *r, const BIGNUM *a, const BIGNUM *p,
         const BIGNUM *m, BN_CTX *ctx, BN_MONT_CTX *m_ctx);
    int RSA_meth_set_bn_mod_exp(RSA_METHOD *rsa,
                                int (*bn_mod_exp) (BIGNUM *r,
                                                   const BIGNUM *a,
                                                   const BIGNUM *p,
                                                   const BIGNUM *m,
                                                   BN_CTX *ctx,
                                                   BN_MONT_CTX *m_ctx));
        /* called at new */
    int (*RSA_meth_get_init(const RSA_METHOD *meth)) (RSA *rsa);
    int RSA_meth_set_init(RSA_METHOD *rsa, int (*init) (RSA *rsa));
        /* called at free */
    int (*RSA_meth_get_finish(const RSA_METHOD *meth)) (RSA *rsa);
    int RSA_meth_set_finish(RSA_METHOD *rsa, int (*finish) (RSA *rsa));
    int (*RSA_meth_get_sign(const RSA_METHOD *meth))
        (int type,
         const unsigned char *m, unsigned int m_length,
         unsigned char *sigret, unsigned int *siglen,
         const RSA *rsa);
    int RSA_meth_set_sign(RSA_METHOD *rsa,
                          int (*sign) (int type, const unsigned char *m,
                                       unsigned int m_length,
                                       unsigned char *sigret, unsigned int *siglen,
                                       const RSA *rsa));
    int (*RSA_meth_get_verify(const RSA_METHOD *meth))
        (int dtype, const unsigned char *m,
         unsigned int m_length, const unsigned char *sigbuf,
         unsigned int siglen, const RSA *rsa);
    int RSA_meth_set_verify(RSA_METHOD *rsa,
                            int (*verify) (int dtype, const unsigned char *m,
                                           unsigned int m_length,
                                           const unsigned char *sigbuf,
                                           unsigned int siglen, const RSA *rsa));
    int (*RSA_meth_get_keygen(const RSA_METHOD *meth))
        (RSA *rsa, int bits, BIGNUM *e, BN_GENCB *cb);
    int RSA_meth_set_keygen(RSA_METHOD *rsa,
                            int (*keygen) (RSA *rsa, int bits, BIGNUM *e,
                                           BN_GENCB *cb));

# DESCRIPTION

The **RSA\_METHOD** type is a structure used for the provision of custom
RSA implementations. It provides a set of of functions used by OpenSSL
for the implementation of the various RSA capabilities. See the [rsa](https://metacpan.org/pod/rsa)
page for more information.

RSA\_meth\_new() creates a new **RSA\_METHOD** structure. It should be
given a unique **name** and a set of **flags**. The **name** should be a
NULL terminated string, which will be duplicated and stored in the
**RSA\_METHOD** object. It is the callers responsibility to free the
original string. The flags will be used during the construction of a
new **RSA** object based on this **RSA\_METHOD**. Any new **RSA** object
will have those flags set by default.

RSA\_meth\_dup() creates a duplicate copy of the **RSA\_METHOD** object
passed as a parameter. This might be useful for creating a new
**RSA\_METHOD** based on an existing one, but with some differences.

RSA\_meth\_free() destroys an **RSA\_METHOD** structure and frees up any
memory associated with it.

RSA\_meth\_get0\_name() will return a pointer to the name of this
RSA\_METHOD. This is a pointer to the internal name string and so
should not be freed by the caller. RSA\_meth\_set1\_name() sets the name
of the RSA\_METHOD to **name**. The string is duplicated and the copy is
stored in the RSA\_METHOD structure, so the caller remains responsible
for freeing the memory associated with the name.

RSA\_meth\_get\_flags() returns the current value of the flags associated
with this RSA\_METHOD. RSA\_meth\_set\_flags() provides the ability to set
these flags.

The functions RSA\_meth\_get0\_app\_data() and RSA\_meth\_set0\_app\_data()
provide the ability to associate implementation specific data with the
RSA\_METHOD. It is the application's responsibility to free this data
before the RSA\_METHOD is freed via a call to RSA\_meth\_free().

RSA\_meth\_get\_sign() and RSA\_meth\_set\_sign() get and set the function
used for creating an RSA signature respectively. This function will be
called in response to the application calling RSA\_sign(). The
parameters for the function have the same meaning as for RSA\_sign().

RSA\_meth\_get\_verify() and RSA\_meth\_set\_verify() get and set the
function used for verifying an RSA signature respectively. This
function will be called in response to the application calling
RSA\_verify(). The parameters for the function have the same meaning as
for RSA\_verify().

RSA\_meth\_get\_mod\_exp() and RSA\_meth\_set\_mod\_exp() get and set the
function used for CRT computations.

RSA\_meth\_get\_bn\_mod\_exp() and RSA\_meth\_set\_bn\_mod\_exp() get and set
the function used for CRT computations, specifically the following
value:

    r = a ^ p mod m

Both the mod\_exp() and bn\_mod\_exp() functions are called by the
default OpenSSL method during encryption, decryption, signing and
verification.

RSA\_meth\_get\_init() and RSA\_meth\_set\_init() get and set the function
used for creating a new RSA instance respectively. This function will
be called in response to the application calling RSA\_new() (if the
current default RSA\_METHOD is this one) or RSA\_new\_method(). The
RSA\_new() and RSA\_new\_method() functions will allocate the memory for
the new RSA object, and a pointer to this newly allocated structure
will be passed as a parameter to the function. This function may be
NULL.

RSA\_meth\_get\_finish() and RSA\_meth\_set\_finish() get and set the
function used for destroying an instance of an RSA object respectively.
This function will be called in response to the application calling
RSA\_free(). A pointer to the RSA to be destroyed is passed as a
parameter. The destroy function should be used for RSA implementation
specific clean up. The memory for the RSA itself should not be freed
by this function. This function may be NULL.

RSA\_meth\_get\_keygen() and RSA\_meth\_set\_keygen() get and set the
function used for generating a new RSA key pair respectively. This
function will be called in response to the application calling
RSA\_generate\_key(). The parameter for the function has the same
meaning as for RSA\_generate\_key().

RSA\_meth\_get\_pub\_enc(), RSA\_meth\_set\_pub\_enc(),
RSA\_meth\_get\_pub\_dec(), RSA\_meth\_set\_pub\_dec(),
RSA\_meth\_get\_priv\_enc(), RSA\_meth\_set\_priv\_enc(),
RSA\_meth\_get\_priv\_dec(), RSA\_meth\_set\_priv\_dec() get and set the
functions used for public and private key encryption and decryption.
These functions will be called in response to the application calling
RSA\_public\_encrypt(), RSA\_private\_decrypt(), RSA\_private\_encrypt() and
RSA\_public\_decrypt() and take the same parameters as those.

# RETURN VALUES

RSA\_meth\_new() and RSA\_meth\_dup() return the newly allocated
RSA\_METHOD object or NULL on failure.

RSA\_meth\_get0\_name() and RSA\_meth\_get\_flags() return the name and
flags associated with the RSA\_METHOD respectively.

All other RSA\_meth\_get\_\*() functions return the appropriate function
pointer that has been set in the RSA\_METHOD, or NULL if no such
pointer has yet been set.

RSA\_meth\_set1\_name and all RSA\_meth\_set\_\*() functions return 1 on
success or 0 on failure.

# SEE ALSO

[RSA\_new(3)](http://man.he.net/man3/RSA_new), [RSA\_generate\_key(3)](http://man.he.net/man3/RSA_generate_key), [RSA\_sign(3)](http://man.he.net/man3/RSA_sign),
[RSA\_set\_method(3)](http://man.he.net/man3/RSA_set_method), [RSA\_size(3)](http://man.he.net/man3/RSA_size), [RSA\_get0\_key(3)](http://man.he.net/man3/RSA_get0_key)

# HISTORY

The functions described here were added in OpenSSL version 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
