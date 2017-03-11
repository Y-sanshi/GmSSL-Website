# NAME

BIO\_get\_ex\_new\_index, BIO\_set\_ex\_data, BIO\_get\_ex\_data,
ENGINE\_get\_ex\_new\_index, ENGINE\_set\_ex\_data, ENGINE\_get\_ex\_data,
UI\_get\_ex\_new\_index, UI\_set\_ex\_data, UI\_get\_ex\_data,
X509\_get\_ex\_new\_index, X509\_set\_ex\_data, X509\_get\_ex\_data,
X509\_STORE\_get\_ex\_new\_index, X509\_STORE\_set\_ex\_data, X509\_STORE\_get\_ex\_data,
X509\_STORE\_CTX\_get\_ex\_new\_index, X509\_STORE\_CTX\_set\_ex\_data, X509\_STORE\_CTX\_get\_ex\_data,
DH\_get\_ex\_new\_index, DH\_set\_ex\_data, DH\_get\_ex\_data,
DSA\_get\_ex\_new\_index, DSA\_set\_ex\_data, DSA\_get\_ex\_data,
ECDH\_get\_ex\_new\_index, ECDH\_set\_ex\_data, ECDH\_get\_ex\_data,
ECDSA\_get\_ex\_new\_index, ECDSA\_set\_ex\_data, ECDSA\_get\_ex\_data,
RSA\_get\_ex\_new\_index, RSA\_set\_ex\_data, RSA\_get\_ex\_data
\- application-specific data

# SYNOPSIS

    #include <openssl/x509.h>

    int TYPE_get_ex_new_index(long argl, void *argp,
                   CRYPTO_EX_new *new_func,
                   CRYPTO_EX_dup *dup_func,
                   CRYPTO_EX_free *free_func);

    int TYPE_set_ex_data(TYPE *d, int idx, void *arg);

    void *TYPE_get_ex_data(TYPE *d, int idx);

# DESCRIPTION

In the description here, _TYPE_ is used a placeholder
for any of the OpenSSL datatypes listed in
[CRYPTO\_get\_ex\_new\_index(3)](http://man.he.net/man3/CRYPTO_get_ex_new_index).

These functions handle application-specific data for OpenSSL data
structures.

TYPE\_get\_new\_ex\_index() is a macro that calls CRYPTO\_get\_ex\_new\_index()
with the correct **index** value.

TYPE\_set\_ex\_data() is a function that calls CRYPTO\_set\_ex\_data() with
an offset into the opaque exdata part of the TYPE object.

TYPE\_get\_ex\_data() is a function that calls CRYPTO\_get\_ex\_data() with an
an offset into the opaque exdata part of the TYPE object.

# SEE ALSO

[CRYPTO\_get\_ex\_new\_index(3)](http://man.he.net/man3/CRYPTO_get_ex_new_index).

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
