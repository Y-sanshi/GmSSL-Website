# NAME

X509\_STORE\_get0\_param, X509\_STORE\_set1\_param,
X509\_STORE\_get0\_objects - X509\_STORE setter and getter functions

# SYNOPSIS

    #include <openssl/x509_vfy.h>

    X509_VERIFY_PARAM *X509_STORE_get0_param(X509_STORE *ctx);
    int X509_STORE_set1_param(X509_STORE *ctx, X509_VERIFY_PARAM *pm);
    STACK_OF(X509_OBJECT) *X509_STORE_get0_objects(X509_STORE *ctx);

# DESCRIPTION

X509\_STORE\_set1\_param() sets the verification parameters
to **pm** for **ctx**.

X509\_STORE\_get0\_param() retrieves an internal pointer to the verification
parameters for **ctx**. The returned pointer must not be freed by the
calling application

X509\_STORE\_get0\_objects() retrieve an internal pointer to the store's
X509 object cache. The cache contains **X509** and **X509\_CRL** objects. The
returned pointer must not be freed by the calling application.

# RETURN VALUES

X509\_STORE\_get0\_param() returns a pointer to an
**X509\_VERIFY\_PARAM** structure.

X509\_STORE\_set1\_param() returns 1 for success and 0 for failure.

X509\_STORE\_get0\_objects() returns a pointer to a stack of **X509\_OBJECT**.

# SEE ALSO

[X509\_STORE\_new(3)](http://man.he.net/man3/X509_STORE_new)

# HISTORY

**X509\_STORE\_get0\_param** and **X509\_STORE\_get0\_objects** were added in
OpenSSL version 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
