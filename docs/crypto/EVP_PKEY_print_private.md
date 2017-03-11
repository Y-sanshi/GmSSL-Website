# NAME

EVP\_PKEY\_print\_public, EVP\_PKEY\_print\_private, EVP\_PKEY\_print\_params - public key algorithm printing routines

# SYNOPSIS

    #include <openssl/evp.h>

    int EVP_PKEY_print_public(BIO *out, const EVP_PKEY *pkey,
                                   int indent, ASN1_PCTX *pctx);
    int EVP_PKEY_print_private(BIO *out, const EVP_PKEY *pkey,
                                   int indent, ASN1_PCTX *pctx);
    int EVP_PKEY_print_params(BIO *out, const EVP_PKEY *pkey,
                                   int indent, ASN1_PCTX *pctx);

# DESCRIPTION

The functions EVP\_PKEY\_print\_public(), EVP\_PKEY\_print\_private() and
EVP\_PKEY\_print\_params() print out the public, private or parameter components
of key **pkey** respectively. The key is sent to BIO **out** in human readable
form. The parameter **indent** indicated how far the printout should be indented.

The **pctx** parameter allows the print output to be finely tuned by using
ASN1 printing options. If **pctx** is set to NULL then default values will
be used.

# NOTES

Currently no public key algorithms include any options in the **pctx** parameter
parameter.

If the key does not include all the components indicated by the function then
only those contained in the key will be printed. For example passing a public
key to EVP\_PKEY\_print\_private() will only print the public components.

# RETURN VALUES

These functions all return 1 for success and 0 or a negative value for failure.
In particular a return value of -2 indicates the operation is not supported by
the public key algorithm.

# SEE ALSO

[EVP\_PKEY\_CTX\_new(3)](http://man.he.net/man3/EVP_PKEY_CTX_new),
[EVP\_PKEY\_keygen(3)](http://man.he.net/man3/EVP_PKEY_keygen)

# HISTORY

These functions were first added to OpenSSL 1.0.0.

# COPYRIGHT

Copyright 2006-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
