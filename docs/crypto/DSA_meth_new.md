# NAME

DSA\_meth\_new, DSA\_meth\_free, DSA\_meth\_dup, DSA\_meth\_get0\_name,
DSA\_meth\_set1\_name, DSA\_meth\_get\_flags, DSA\_meth\_set\_flags,
DSA\_meth\_get0\_app\_data, DSA\_meth\_set0\_app\_data, DSA\_meth\_get\_sign,
DSA\_meth\_set\_sign, DSA\_meth\_get\_sign\_setup, DSA\_meth\_set\_sign\_setup,
DSA\_meth\_get\_verify, DSA\_meth\_set\_verify, DSA\_meth\_get\_mod\_exp,
DSA\_meth\_set\_mod\_exp, DSA\_meth\_get\_bn\_mod\_exp, DSA\_meth\_set\_bn\_mod\_exp,
DSA\_meth\_get\_init, DSA\_meth\_set\_init, DSA\_meth\_get\_finish, DSA\_meth\_set\_finish,
DSA\_meth\_get\_paramgen, DSA\_meth\_set\_paramgen, DSA\_meth\_get\_keygen,
DSA\_meth\_set\_keygen  - Routines to build up DSA methods

# SYNOPSIS

    #include <openssl/dsa.h>

    DSA_METHOD *DSA_meth_new(const char *name, int flags);
    void DSA_meth_free(DSA_METHOD *dsam);
    DSA_METHOD *DSA_meth_dup(const DSA_METHOD *meth);
    const char *DSA_meth_get0_name(const DSA_METHOD *dsam);
    int DSA_meth_set1_name(DSA_METHOD *dsam, const char *name);
    int DSA_meth_get_flags(DSA_METHOD *dsam);
    int DSA_meth_set_flags(DSA_METHOD *dsam, int flags);
    void *DSA_meth_get0_app_data(const DSA_METHOD *dsam);
    int DSA_meth_set0_app_data(DSA_METHOD *dsam, void *app_data);
    DSA_SIG *(*DSA_meth_get_sign(const DSA_METHOD *dsam))
            (const unsigned char *, int, DSA *);
    int DSA_meth_set_sign(DSA_METHOD *dsam,
                          DSA_SIG *(*sign) (const unsigned char *, int, DSA *));
    int (*DSA_meth_get_sign_setup(const DSA_METHOD *dsam))
            (DSA *, BN_CTX *, BIGNUM **, BIGNUM **);
    int DSA_meth_set_sign_setup(DSA_METHOD *dsam,
            int (*sign_setup) (DSA *, BN_CTX *, BIGNUM **, BIGNUM **));
    int (*DSA_meth_get_verify(const DSA_METHOD *dsam))
            (const unsigned char *, int , DSA_SIG *, DSA *);
    int DSA_meth_set_verify(DSA_METHOD *dsam,
        int (*verify) (const unsigned char *, int, DSA_SIG *, DSA *));
    int (*DSA_meth_get_mod_exp(const DSA_METHOD *dsam))
           (DSA *dsa, BIGNUM *rr, BIGNUM *a1, BIGNUM *p1, BIGNUM *a2, BIGNUM *p2,
            BIGNUM *m, BN_CTX *ctx, BN_MONT_CTX *in_mont);
    int DSA_meth_set_mod_exp(DSA_METHOD *dsam,
        int (*mod_exp) (DSA *dsa, BIGNUM *rr, BIGNUM *a1, BIGNUM *p1, BIGNUM *a2,
                        BIGNUM *p2, BIGNUM *m, BN_CTX *ctx, BN_MONT_CTX *mont));
    int (*DSA_meth_get_bn_mod_exp(const DSA_METHOD *dsam))
        (DSA *dsa, BIGNUM *r, BIGNUM *a, const BIGNUM *p, const BIGNUM *m,
         BN_CTX *ctx, BN_MONT_CTX *mont);
    int DSA_meth_set_bn_mod_exp(DSA_METHOD *dsam,
        int (*bn_mod_exp) (DSA *dsa, BIGNUM *r, BIGNUM *a, const BIGNUM *p,
                           const BIGNUM *m, BN_CTX *ctx, BN_MONT_CTX *mont));
    int (*DSA_meth_get_init(const DSA_METHOD *dsam))(DSA *);
    int DSA_meth_set_init(DSA_METHOD *dsam, int (*init)(DSA *));
    int (*DSA_meth_get_finish(const DSA_METHOD *dsam)) (DSA *);
    int DSA_meth_set_finish(DSA_METHOD *dsam, int (*finish) (DSA *));
    int (*DSA_meth_get_paramgen(const DSA_METHOD *dsam))
            (DSA *, int, const unsigned char *, int, int *, unsigned long *,
             BN_GENCB *);
    int DSA_meth_set_paramgen(DSA_METHOD *dsam,
            int (*paramgen) (DSA *, int, const unsigned char *, int, int *,
                             unsigned long *, BN_GENCB *));
    int (*DSA_meth_get_keygen(const DSA_METHOD *dsam)) (DSA *);
    int DSA_meth_set_keygen(DSA_METHOD *dsam, int (*keygen) (DSA *));

# DESCRIPTION

The **DSA\_METHOD** type is a structure used for the provision of custom DSA
implementations. It provides a set of of functions used by OpenSSL for the
implementation of the various DSA capabilities. See the [dsa](https://metacpan.org/pod/dsa) page for more
information.

DSA\_meth\_new() creates a new **DSA\_METHOD** structure. It should be given a
unique **name** and a set of **flags**. The **name** should be a NULL terminated
string, which will be duplicated and stored in the **DSA\_METHOD** object. It is
the callers responsibility to free the original string. The flags will be used
during the construction of a new **DSA** object based on this **DSA\_METHOD**. Any
new **DSA** object will have those flags set by default.

DSA\_meth\_dup() creates a duplicate copy of the **DSA\_METHOD** object passed as a
parameter. This might be useful for creating a new **DSA\_METHOD** based on an
existing one, but with some differences.

DSA\_meth\_free() destroys a **DSA\_METHOD** structure and frees up any memory
associated with it.

DSA\_meth\_get0\_name() will return a pointer to the name of this DSA\_METHOD. This
is a pointer to the internal name string and so should not be freed by the
caller. DSA\_meth\_set1\_name() sets the name of the DSA\_METHOD to **name**. The
string is duplicated and the copy is stored in the DSA\_METHOD structure, so the
caller remains responsible for freeing the memory associated with the name.

DSA\_meth\_get\_flags() returns the current value of the flags associated with this
DSA\_METHOD. DSA\_meth\_set\_flags() provides the ability to set these flags.

The functions DSA\_meth\_get0\_app\_data() and DSA\_meth\_set0\_app\_data() provide the
ability to associate implementation specific data with the DSA\_METHOD. It is
the application's responsibility to free this data before the DSA\_METHOD is
freed via a call to DSA\_meth\_free().

DSA\_meth\_get\_sign() and DSA\_meth\_set\_sign() get and set the function used for
creating a DSA signature respectively. This function will be
called in response to the application calling DSA\_do\_sign() (or DSA\_sign()). The
parameters for the function have the same meaning as for DSA\_do\_sign().

DSA\_meth\_get\_sign\_setup() and DSA\_meth\_set\_sign\_setup() get and set the function
used for precalculating the DSA signature values **k^-1** and **r**. This function
will be called in response to the application calling DSA\_sign\_setup(). The
parameters for the function have the same meaning as for DSA\_sign\_setup().

DSA\_meth\_get\_verify() and DSA\_meth\_set\_verify() get and set the function used
for verifying a DSA signature respectively. This function will be called in
response to the application calling DSA\_do\_verify() (or DSA\_verify()). The
parameters for the function have the same meaning as for DSA\_do\_verify().

DSA\_meth\_get\_mod\_exp() and DSA\_meth\_set\_mod\_exp() get and set the function used
for computing the following value:

    rr = a1^p1 * a2^p2 mod m

This function will be called by the default OpenSSL method during verification
of a DSA signature. The result is stored in the **rr** parameter. This function
may be NULL.

DSA\_meth\_get\_bn\_mod\_exp() and DSA\_meth\_set\_bn\_mod\_exp() get and set the function
used for computing the following value:

    r = a ^ p mod m

This function will be called by the default OpenSSL function for
DSA\_sign\_setup(). The result is stored in the **r** parameter. This function
may be NULL.

DSA\_meth\_get\_init() and DSA\_meth\_set\_init() get and set the function used
for creating a new DSA instance respectively. This function will be
called in response to the application calling DSA\_new() (if the current default
DSA\_METHOD is this one) or DSA\_new\_method(). The DSA\_new() and DSA\_new\_method()
functions will allocate the memory for the new DSA object, and a pointer to this
newly allocated structure will be passed as a parameter to the function. This
function may be NULL.

DSA\_meth\_get\_finish() and DSA\_meth\_set\_finish() get and set the function used
for destroying an instance of a DSA object respectively. This function will be
called in response to the application calling DSA\_free(). A pointer to the DSA
to be destroyed is passed as a parameter. The destroy function should be used
for DSA implementation specific clean up. The memory for the DSA itself should
not be freed by this function. This function may be NULL.

DSA\_meth\_get\_paramgen() and DSA\_meth\_set\_paramgen() get and set the function
used for generating DSA parameters respectively. This function will be called in
response to the application calling DSA\_generate\_parameters\_ex() (or
DSA\_generate\_parameters()). The parameters for the function have the same
meaning as for DSA\_generate\_parameters\_ex().

DSA\_meth\_get\_keygen() and DSA\_meth\_set\_keygen() get and set the function
used for generating a new DSA key pair respectively. This function will be
called in response to the application calling DSA\_generate\_key(). The parameter
for the function has the same meaning as for DSA\_generate\_key().

# RETURN VALUES

DSA\_meth\_new() and DSA\_meth\_dup() return the newly allocated DSA\_METHOD object
or NULL on failure.

DSA\_meth\_get0\_name() and DSA\_meth\_get\_flags() return the name and flags
associated with the DSA\_METHOD respectively.

All other DSA\_meth\_get\_\*() functions return the appropriate function pointer
that has been set in the DSA\_METHOD, or NULL if no such pointer has yet been
set.

DSA\_meth\_set1\_name() and all DSA\_meth\_set\_\*() functions return 1 on success or
0 on failure.

# SEE ALSO

[dsa(3)](http://man.he.net/man3/dsa), [DSA\_new(3)](http://man.he.net/man3/DSA_new), [DSA\_generate\_parameters(3)](http://man.he.net/man3/DSA_generate_parameters), [DSA\_generate\_key(3)](http://man.he.net/man3/DSA_generate_key),
[DSA\_dup\_DH(3)](http://man.he.net/man3/DSA_dup_DH), [DSA\_do\_sign(3)](http://man.he.net/man3/DSA_do_sign), [DSA\_set\_method(3)](http://man.he.net/man3/DSA_set_method), [DSA\_SIG\_new(3)](http://man.he.net/man3/DSA_SIG_new),
[DSA\_sign(3)](http://man.he.net/man3/DSA_sign), [DSA\_size(3)](http://man.he.net/man3/DSA_size), [DSA\_get0\_pqg(3)](http://man.he.net/man3/DSA_get0_pqg)

# HISTORY

The functions described here were added in OpenSSL version 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
