# NAME

EVP\_CIPHER\_CTX\_get\_cipher\_data, EVP\_CIPHER\_CTX\_set\_cipher\_data - Routines to
inspect and modify EVP\_CIPHER\_CTX objects

# SYNOPSIS

    #include <openssl/evp.h>

    void *EVP_CIPHER_CTX_get_cipher_data(const EVP_CIPHER_CTX *ctx);
    void *EVP_CIPHER_CTX_set_cipher_data(EVP_CIPHER_CTX *ctx, void *cipher_data);

# DESCRIPTION

The EVP\_CIPHER\_CTX\_get\_cipher\_data() function returns a pointer to the cipher
data relevant to EVP\_CIPHER\_CTX. The contents of this data is specific to the
particular implementation of the cipher. For example this data can be used by
engines to store engine specific information. The data is automatically
allocated and freed by OpenSSL, so applications and engines should not normally
free this directly (but see below).

The EVP\_CIPHER\_CTX\_set\_cipher\_data() function allows an application or engine to
replace the cipher data with new data. A pointer to any existing cipher data is
returned from this function. If the old data is no longer required then it
should be freed through a call to OPENSSL\_free().

# RETURN VALUES

The EVP\_CIPHER\_CTX\_get\_cipher\_data() function returns a pointer to the current
cipher data for the EVP\_CIPHER\_CTX.

The EVP\_CIPHER\_CTX\_set\_cipher\_data() function returns a pointer to the old
cipher data for the EVP\_CIPHER\_CTX.

# HISTORY

The EVP\_CIPHER\_CTX\_get\_cipher\_data() and EVP\_CIPHER\_CTX\_set\_cipher\_data()
functions were added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
