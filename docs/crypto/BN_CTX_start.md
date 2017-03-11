# NAME

BN\_CTX\_start, BN\_CTX\_get, BN\_CTX\_end - use temporary BIGNUM variables

# SYNOPSIS

    #include <openssl/bn.h>

    void BN_CTX_start(BN_CTX *ctx);

    BIGNUM *BN_CTX_get(BN_CTX *ctx);

    void BN_CTX_end(BN_CTX *ctx);

# DESCRIPTION

These functions are used to obtain temporary **BIGNUM** variables from
a **BN\_CTX** (which can been created by using [BN\_CTX\_new(3)](http://man.he.net/man3/BN_CTX_new))
in order to save the overhead of repeatedly creating and
freeing **BIGNUM**s in functions that are called from inside a loop.

A function must call BN\_CTX\_start() first. Then, BN\_CTX\_get() may be
called repeatedly to obtain temporary **BIGNUM**s. All BN\_CTX\_get()
calls must be made before calling any other functions that use the
**ctx** as an argument.

Finally, BN\_CTX\_end() must be called before returning from the function.
When BN\_CTX\_end() is called, the **BIGNUM** pointers obtained from
BN\_CTX\_get() become invalid.

# RETURN VALUES

BN\_CTX\_start() and BN\_CTX\_end() return no values.

BN\_CTX\_get() returns a pointer to the **BIGNUM**, or **NULL** on error.
Once BN\_CTX\_get() has failed, the subsequent calls will return **NULL**
as well, so it is sufficient to check the return value of the last
BN\_CTX\_get() call. In case of an error, an error code is set, which
can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# SEE ALSO

[BN\_CTX\_new(3)](http://man.he.net/man3/BN_CTX_new)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
