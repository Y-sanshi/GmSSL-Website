# NAME

X509\_STORE\_set\_lookup\_crls\_cb,
X509\_STORE\_set\_verify\_func,
X509\_STORE\_get\_cleanup,
X509\_STORE\_set\_cleanup,
X509\_STORE\_get\_lookup\_crls,
X509\_STORE\_set\_lookup\_crls,
X509\_STORE\_get\_lookup\_certs,
X509\_STORE\_set\_lookup\_certs,
X509\_STORE\_get\_check\_policy,
X509\_STORE\_set\_check\_policy,
X509\_STORE\_get\_cert\_crl,
X509\_STORE\_set\_cert\_crl,
X509\_STORE\_get\_check\_crl,
X509\_STORE\_set\_check\_crl,
X509\_STORE\_get\_get\_crl,
X509\_STORE\_set\_get\_crl,
X509\_STORE\_get\_check\_revocation,
X509\_STORE\_set\_check\_revocation,
X509\_STORE\_get\_check\_issued,
X509\_STORE\_set\_check\_issued,
X509\_STORE\_get\_get\_issuer,
X509\_STORE\_set\_get\_issuer,
X509\_STORE\_CTX\_get\_verify,
X509\_STORE\_set\_verify,
X509\_STORE\_get\_verify\_cb,
X509\_STORE\_set\_verify\_cb\_func, X509\_STORE\_set\_verify\_cb,
X509\_STORE\_CTX\_cert\_crl\_fn, X509\_STORE\_CTX\_check\_crl\_fn,
X509\_STORE\_CTX\_check\_issued\_fn, X509\_STORE\_CTX\_check\_policy\_fn,
X509\_STORE\_CTX\_check\_revocation\_fn, X509\_STORE\_CTX\_cleanup\_fn
X509\_STORE\_CTX\_get\_crl\_fn, X509\_STORE\_CTX\_get\_issuer\_fn,
X509\_STORE\_CTX\_lookup\_certs\_fn, X509\_STORE\_CTX\_lookup\_crls\_fn,
X509\_STORE\_CTX\_verify\_cb, X509\_STORE\_CTX\_verify\_fn,
\- set verification callback

# SYNOPSIS

    #include <openssl/x509_vfy.h>

    typedef int (*X509_STORE_CTX_verify_cb)(int ok, X509_STORE_CTX *ctx);
    typedef int (*X509_STORE_CTX_verify_fn)(X509_STORE_CTX *ctx);
    typedef int (*X509_STORE_CTX_get_issuer_fn)(X509 **issuer,
                                                X509_STORE_CTX *ctx, X509 *x);
    typedef int (*X509_STORE_CTX_check_issued_fn)(X509_STORE_CTX *ctx,
                                                  X509 *x, X509 *issuer);
    typedef int (*X509_STORE_CTX_check_revocation_fn)(X509_STORE_CTX *ctx);
    typedef int (*X509_STORE_CTX_get_crl_fn)(X509_STORE_CTX *ctx,
                                             X509_CRL **crl, X509 *x);
    typedef int (*X509_STORE_CTX_check_crl_fn)(X509_STORE_CTX *ctx, X509_CRL *crl);
    typedef int (*X509_STORE_CTX_cert_crl_fn)(X509_STORE_CTX *ctx,
                                              X509_CRL *crl, X509 *x);
    typedef int (*X509_STORE_CTX_check_policy_fn)(X509_STORE_CTX *ctx);
    typedef STACK_OF(X509) *(*X509_STORE_CTX_lookup_certs_fn)(X509_STORE_CTX *ctx,
                                                              X509_NAME *nm);
    typedef STACK_OF(X509_CRL) *(*X509_STORE_CTX_lookup_crls_fn)(X509_STORE_CTX *ctx,
                                                                 X509_NAME *nm);
    typedef int (*X509_STORE_CTX_cleanup_fn)(X509_STORE_CTX *ctx);

    void X509_STORE_set_verify_cb(X509_STORE *ctx,
                                  X509_STORE_CTX_verify_cb verify_cb);
    X509_STORE_CTX_verify_cb X509_STORE_get_verify_cb(X509_STORE_CTX *ctx);

    void X509_STORE_set_verify(X509_STORE *ctx, X509_STORE_CTX_verify_fn verify);
    X509_STORE_CTX_verify_fn X509_STORE_CTX_get_verify(X509_STORE_CTX *ctx);

    void X509_STORE_set_get_issuer(X509_STORE *ctx,
                                   X509_STORE_CTX_get_issuer_fn get_issuer);
    X509_STORE_CTX_get_issuer_fn X509_STORE_get_get_issuer(X509_STORE_CTX *ctx);

    void X509_STORE_set_check_issued(X509_STORE *ctx,
                                     X509_STORE_CTX_check_issued_fn check_issued);
    X509_STORE_CTX_check_issued_fn X509_STORE_get_check_issued(X509_STORE_CTX *ctx);

    void X509_STORE_set_check_revocation(X509_STORE *ctx,
                                         X509_STORE_CTX_check_revocation_fn check_revocation);
    X509_STORE_CTX_check_revocation_fn X509_STORE_get_check_revocation(X509_STORE_CTX *ctx);

    void X509_STORE_set_get_crl(X509_STORE *ctx,
                                X509_STORE_CTX_get_crl_fn get_crl);
    X509_STORE_CTX_get_crl_fn X509_STORE_get_get_crl(X509_STORE_CTX *ctx);

    void X509_STORE_set_check_crl(X509_STORE *ctx,
                                  X509_STORE_CTX_check_crl_fn check_crl);
    X509_STORE_CTX_check_crl_fn X509_STORE_get_check_crl(X509_STORE_CTX *ctx);

    void X509_STORE_set_cert_crl(X509_STORE *ctx,
                                 X509_STORE_CTX_cert_crl_fn cert_crl);
    X509_STORE_CTX_cert_crl_fn X509_STORE_get_cert_crl(X509_STORE_CTX *ctx);

    void X509_STORE_set_check_policy(X509_STORE *ctx,
                                     X509_STORE_CTX_check_policy_fn check_policy);
    X509_STORE_CTX_check_policy_fn X509_STORE_get_check_policy(X509_STORE_CTX *ctx);

    void X509_STORE_set_lookup_certs(X509_STORE *ctx,
                                     X509_STORE_CTX_lookup_certs_fn lookup_certs);
    X509_STORE_CTX_lookup_certs_fn X509_STORE_get_lookup_certs(X509_STORE_CTX *ctx);

    void X509_STORE_set_lookup_crls(X509_STORE *ctx,
                                    X509_STORE_CTX_lookup_crls_fn lookup_crls);
    X509_STORE_CTX_lookup_crls_fn X509_STORE_get_lookup_crls(X509_STORE_CTX *ctx);

    void X509_STORE_set_cleanup(X509_STORE *ctx,
                                X509_STORE_CTX_cleanup_fn cleanup);
    X509_STORE_CTX_cleanup_fn X509_STORE_get_cleanup(X509_STORE_CTX *ctx);

    /* Aliases */
    void X509_STORE_set_verify_cb_func(X509_STORE *st,
                                       X509_STORE_CTX_verify_cb verify_cb);
    void X509_STORE_set_verify_func(X509_STORE *ctx,
                                    X509_STORE_CTX_verify_fn verify);
    void X509_STORE_set_lookup_crls_cb(X509_STORE *ctx,
                                       X509_STORE_CTX_lookup_crls_fn lookup_crls);

# DESCRIPTION

X509\_STORE\_set\_verify\_cb() sets the verification callback of **ctx** to
**verify\_cb** overwriting the previous callback.
The callback assigned with this function becomes a default for the one
that can be assigned directly to the corresponding **X509\_STORE\_CTX**,
please see [X509\_STORE\_CTX\_set\_verify\_cb(3)](http://man.he.net/man3/X509_STORE_CTX_set_verify_cb) for further information.

X509\_STORE\_set\_verify() sets the final chain verification function for
**ctx** to **verify**.
Its purpose is to go through the chain of certificates and check that
all signatures are valid and that the current time is within the
limits of each certificate's first and last validity time.
The final chain verification functions must return 0 on failure and 1
on success.
_If no chain verification function is provided, the internal default
function will be used instead._

X509\_STORE\_set\_get\_issuer() sets the function to get the issuer
certificate that verifies the given certificate **x**.
When found, the issuer certificate must be assigned to **\*issuer**.
This function must return 0 on failure and 1 on success.
_If no function to get the issuer is provided, the internal default
function will be used instead._

X509\_STORE\_set\_check\_issued() sets the function to check that a given
certificate **x** is issued with the issuer certificate **issuer**.
This function must return 0 on failure (among others if **x** hasn't
been issued with **issuer**) and 1 on success.
_If no function to get the issuer is provided, the internal default
function will be used instead._

X509\_STORE\_set\_check\_revocation() sets the revocation checking
function.
Its purpose is to look through the final chain and check the
revocation status for each certificate.
It must return 0 on failure and 1 on success.
_If no function to get the issuer is provided, the internal default
function will be used instead._

X509\_STORE\_set\_get\_crl() sets the function to get the crl for a given
certificate **x**.
When found, the crl must be assigned to **\*crl**.
This function must return 0 on failure and 1 on success.
_If no function to get the issuer is provided, the internal default
function will be used instead._

X509\_STORE\_set\_check\_crl() sets the function to check the validity of
the given **crl**.
This function must return 0 on failure and 1 on success.
_If no function to get the issuer is provided, the internal default
function will be used instead._

X509\_STORE\_set\_cert\_crl() sets the function to check the revocation
status of the given certificate **x** against the given **crl**.
This function must return 0 on failure and 1 on success.
_If no function to get the issuer is provided, the internal default
function will be used instead._

X509\_STORE\_set\_check\_policy() sets the function to check the policies
of all the certificates in the final chain..
This function must return 0 on failure and 1 on success.
_If no function to get the issuer is provided, the internal default
function will be used instead._

X509\_STORE\_set\_lookup\_certs() and X509\_STORE\_set\_lookup\_crls() set the
functions to look up all the certs or all the CRLs that match the
given name **nm**.
These functions return NULL on failure and a pointer to a stack of
certificates (**X509**) or to a stack of CRLs (**X509\_CRL**) on
success.
_If no function to get the issuer is provided, the internal default
function will be used instead._

X509\_STORE\_set\_cleanup() sets the final cleanup function, which is
called when the context (**X509\_STORE\_CTX**) is being torn down.
This function doesn't return any value.
_If no function to get the issuer is provided, the internal default
function will be used instead._

X509\_STORE\_get\_verify\_cb(), X509\_STORE\_CTX\_get\_verify(),
X509\_STORE\_get\_get\_issuer(), X509\_STORE\_get\_check\_issued(),
X509\_STORE\_get\_check\_revocation(), X509\_STORE\_get\_get\_crl(),
X509\_STORE\_get\_check\_crl(), X509\_STORE\_set\_verify(),
X509\_STORE\_set\_get\_issuer(), X509\_STORE\_get\_cert\_crl(),
X509\_STORE\_get\_check\_policy(), X509\_STORE\_get\_lookup\_certs(),
X509\_STORE\_get\_lookup\_crls() and X509\_STORE\_get\_cleanup() all return
the function pointer assigned with X509\_STORE\_set\_check\_issued(),
X509\_STORE\_set\_check\_revocation(), X509\_STORE\_set\_get\_crl(),
X509\_STORE\_set\_check\_crl(), X509\_STORE\_set\_cert\_crl(),
X509\_STORE\_set\_check\_policy(), X509\_STORE\_set\_lookup\_certs(),
X509\_STORE\_set\_lookup\_crls() and X509\_STORE\_set\_cleanup(), or NULL if
no assignment has been made.

X509\_STORE\_set\_verify\_cb\_func(), X509\_STORE\_set\_verify\_func() and
X509\_STORE\_set\_lookup\_crls\_cb() are aliases for
X509\_STORE\_set\_verify\_cb(), X509\_STORE\_set\_verify() and
X509\_STORE\_set\_lookup\_crls, available as macros for backward
compatibility.

# NOTES

All the callbacks from a **X509\_STORE** are inherited by the
corresponding **X509\_STORE\_CTX** structure when it is initialized.
See [X509\_STORE\_CTX\_set\_verify\_cb(3)](http://man.he.net/man3/X509_STORE_CTX_set_verify_cb) for further details.

# BUGS

The macro version of this function was the only one available before
OpenSSL 1.0.0.

# RETURN VALUES

The X509\_STORE\_set\_\*() functions do not return a value.

The X509\_STORE\_get\_\*() functions return a pointer of the appropriate
function type.

# SEE ALSO

[X509\_STORE\_CTX\_set\_verify\_cb(3)](http://man.he.net/man3/X509_STORE_CTX_set_verify_cb), [X509\_STORE\_CTX\_get0\_chain(3)](http://man.he.net/man3/X509_STORE_CTX_get0_chain),
[CMS\_verify(3)](http://man.he.net/man3/CMS_verify)

# HISTORY

X509\_STORE\_set\_verify\_cb() was added to OpenSSL 1.0.0.

X509\_STORE\_set\_verify\_cb(), X509\_STORE\_get\_verify\_cb(),
X509\_STORE\_set\_verify(), X509\_STORE\_CTX\_get\_verify(),
X509\_STORE\_set\_get\_issuer(), X509\_STORE\_get\_get\_issuer(),
X509\_STORE\_set\_check\_issued(), X509\_STORE\_get\_check\_issued(),
X509\_STORE\_set\_check\_revocation(), X509\_STORE\_get\_check\_revocation(),
X509\_STORE\_set\_get\_crl(), X509\_STORE\_get\_get\_crl(),
X509\_STORE\_set\_check\_crl(), X509\_STORE\_get\_check\_crl(),
X509\_STORE\_set\_cert\_crl(), X509\_STORE\_get\_cert\_crl(),
X509\_STORE\_set\_check\_policy(), X509\_STORE\_get\_check\_policy(),
X509\_STORE\_set\_lookup\_certs(), X509\_STORE\_get\_lookup\_certs(),
X509\_STORE\_set\_lookup\_crls(), X509\_STORE\_get\_lookup\_crls(),
X509\_STORE\_set\_cleanup() and X509\_STORE\_get\_cleanup() were added in
OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2009-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
