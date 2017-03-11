# NAME

DH\_meth\_new, DH\_meth\_free, DH\_meth\_dup, DH\_meth\_get0\_name, DH\_meth\_set1\_name,
DH\_meth\_get\_flags, DH\_meth\_set\_flags, DH\_meth\_get0\_app\_data,
DH\_meth\_set0\_app\_data, DH\_meth\_get\_generate\_key, DH\_meth\_set\_generate\_key,
DH\_meth\_get\_compute\_key, DH\_meth\_set\_compute\_key, DH\_meth\_get\_bn\_mod\_exp,
DH\_meth\_set\_bn\_mod\_exp, DH\_meth\_get\_init, DH\_meth\_set\_init, DH\_meth\_get\_finish,
DH\_meth\_set\_finish, DH\_meth\_get\_generate\_params,
DH\_meth\_set\_generate\_params - Routines to build up DH methods

# SYNOPSIS

    #include <openssl/dh.h>

    DH_METHOD *DH_meth_new(const char *name, int flags);
    void DH_meth_free(DH_METHOD *dhm);
    DH_METHOD *DH_meth_dup(const DH_METHOD *dhm);
    const char *DH_meth_get0_name(const DH_METHOD *dhm);
    int DH_meth_set1_name(DH_METHOD *dhm, const char *name);
    int DH_meth_get_flags(DH_METHOD *dhm);
    int DH_meth_set_flags(DH_METHOD *dhm, int flags);
    void *DH_meth_get0_app_data(const DH_METHOD *dhm);
    int DH_meth_set0_app_data(DH_METHOD *dhm, void *app_data);
    int (*DH_meth_get_generate_key(const DH_METHOD *dhm)) (DH *);
    int DH_meth_set_generate_key(DH_METHOD *dhm, int (*generate_key) (DH *));
    int (*DH_meth_get_compute_key(const DH_METHOD *dhm))
            (unsigned char *key, const BIGNUM *pub_key, DH *dh);
    int DH_meth_set_compute_key(DH_METHOD *dhm,
            int (*compute_key) (unsigned char *key, const BIGNUM *pub_key, DH *dh));
    int (*DH_meth_get_bn_mod_exp(const DH_METHOD *dhm))
        (const DH *dh, BIGNUM *r, const BIGNUM *a, const BIGNUM *p,
         const BIGNUM *m, BN_CTX *ctx, BN_MONT_CTX *m_ctx);
    int DH_meth_set_bn_mod_exp(DH_METHOD *dhm,
        int (*bn_mod_exp) (const DH *dh, BIGNUM *r, const BIGNUM *a,
                           const BIGNUM *p, const BIGNUM *m, BN_CTX *ctx,
                           BN_MONT_CTX *m_ctx));
    int (*DH_meth_get_init(const DH_METHOD *dhm))(DH *);
    int DH_meth_set_init(DH_METHOD *dhm, int (*init)(DH *));
    int (*DH_meth_get_finish(const DH_METHOD *dhm)) (DH *);
    int DH_meth_set_finish(DH_METHOD *dhm, int (*finish) (DH *));
    int (*DH_meth_get_generate_params(const DH_METHOD *dhm))
            (DH *, int, int, BN_GENCB *);
    int DH_meth_set_generate_params(DH_METHOD *dhm,
            int (*generate_params) (DH *, int, int, BN_GENCB *));

# DESCRIPTION

The **DH\_METHOD** type is a structure used for the provision of custom DH
implementations. It provides a set of of functions used by OpenSSL for the
implementation of the various DH capabilities.

DH\_meth\_new() creates a new **DH\_METHOD** structure. It should be given a
unique **name** and a set of **flags**. The **name** should be a NULL terminated
string, which will be duplicated and stored in the **DH\_METHOD** object. It is
the callers responsibility to free the original string. The flags will be used
during the construction of a new **DH** object based on this **DH\_METHOD**. Any
new **DH** object will have those flags set by default.

DH\_meth\_dup() creates a duplicate copy of the **DH\_METHOD** object passed as a
parameter. This might be useful for creating a new **DH\_METHOD** based on an
existing one, but with some differences.

DH\_meth\_free() destroys a **DH\_METHOD** structure and frees up any memory
associated with it.

DH\_meth\_get0\_name() will return a pointer to the name of this DH\_METHOD. This
is a pointer to the internal name string and so should not be freed by the
caller. DH\_meth\_set1\_name() sets the name of the DH\_METHOD to **name**. The
string is duplicated and the copy is stored in the DH\_METHOD structure, so the
caller remains responsible for freeing the memory associated with the name.

DH\_meth\_get\_flags() returns the current value of the flags associated with this
DH\_METHOD. DH\_meth\_set\_flags() provides the ability to set these flags.

The functions DH\_meth\_get0\_app\_data() and DH\_meth\_set0\_app\_data() provide the
ability to associate implementation specific data with the DH\_METHOD. It is
the application's responsibility to free this data before the DH\_METHOD is
freed via a call to DH\_meth\_free().

DH\_meth\_get\_generate\_key() and DH\_meth\_set\_generate\_key() get and set the
function used for generating a new DH key pair respectively. This function will
be called in response to the application calling DH\_generate\_key(). The
parameter for the function has the same meaning as for DH\_generate\_key().

DH\_meth\_get\_compute\_key() and DH\_meth\_set\_compute\_key() get and set the
function used for computing a new DH shared secret respectively. This function
will be called in response to the application calling DH\_compute\_key(). The
parameters for the function have the same meaning as for DH\_compute\_key().

DH\_meth\_get\_bn\_mod\_exp() and DH\_meth\_set\_bn\_mod\_exp() get and set the function
used for computing the following value:

    r = a ^ p mod m

This function will be called by the default OpenSSL function for
DH\_generate\_key(). The result is stored in the **r** parameter. This function
may be NULL unless using the default generate key function, in which case it
must be present.

DH\_meth\_get\_init() and DH\_meth\_set\_init() get and set the function used
for creating a new DH instance respectively. This function will be
called in response to the application calling DH\_new() (if the current default
DH\_METHOD is this one) or DH\_new\_method(). The DH\_new() and DH\_new\_method()
functions will allocate the memory for the new DH object, and a pointer to this
newly allocated structure will be passed as a parameter to the function. This
function may be NULL.

DH\_meth\_get\_finish() and DH\_meth\_set\_finish() get and set the function used
for destroying an instance of a DH object respectively. This function will be
called in response to the application calling DH\_free(). A pointer to the DH
to be destroyed is passed as a parameter. The destroy function should be used
for DH implementation specific clean up. The memory for the DH itself should
not be freed by this function. This function may be NULL.

DH\_meth\_get\_generate\_params() and DH\_meth\_set\_generate\_params() get and set the
function used for generating DH parameters respectively. This function will be
called in response to the application calling DH\_generate\_parameters\_ex() (or
DH\_generate\_parameters()). The parameters for the function have the same
meaning as for DH\_generate\_parameters\_ex(). This function may be NULL.

# RETURN VALUES

DH\_meth\_new() and DH\_meth\_dup() return the newly allocated DH\_METHOD object
or NULL on failure.

DH\_meth\_get0\_name() and DH\_meth\_get\_flags() return the name and flags
associated with the DH\_METHOD respectively.

All other DH\_meth\_get\_\*() functions return the appropriate function pointer
that has been set in the DH\_METHOD, or NULL if no such pointer has yet been
set.

DH\_meth\_set1\_name() and all DH\_meth\_set\_\*() functions return 1 on success or
0 on failure.

# SEE ALSO

[dh(3)](http://man.he.net/man3/dh), [DH\_new(3)](http://man.he.net/man3/DH_new), [DH\_generate\_parameters(3)](http://man.he.net/man3/DH_generate_parameters), [DH\_generate\_key(3)](http://man.he.net/man3/DH_generate_key),
[DH\_set\_method(3)](http://man.he.net/man3/DH_set_method), [DH\_size(3)](http://man.he.net/man3/DH_size), [DH\_get0\_pqg(3)](http://man.he.net/man3/DH_get0_pqg)

# HISTORY

The functions described here were added in OpenSSL version 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
