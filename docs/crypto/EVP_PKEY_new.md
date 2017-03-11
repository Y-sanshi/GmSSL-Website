# NAME

EVP\_PKEY\_new, EVP\_PKEY\_up\_ref, EVP\_PKEY\_free - private key allocation functions

# SYNOPSIS

    #include <openssl/evp.h>

    EVP_PKEY *EVP_PKEY_new(void);
    int EVP_PKEY_up_ref(EVP_PKEY *key);
    void EVP_PKEY_free(EVP_PKEY *key);

# DESCRIPTION

The EVP\_PKEY\_new() function allocates an empty **EVP\_PKEY** structure which is
used by OpenSSL to store private keys. The reference count is set to **1**.

EVP\_PKEY\_up\_ref() increments the reference count of **key**.

EVP\_PKEY\_free() decrements the reference count of **key** and, if the reference
count is zero, frees it up. If **key** is NULL, nothing is done.

# NOTES

The **EVP\_PKEY** structure is used by various OpenSSL functions which require a
general private key without reference to any particular algorithm.

The structure returned by EVP\_PKEY\_new() is empty. To add a private key to this
empty structure the functions described in [EVP\_PKEY\_set1\_RSA(3)](http://man.he.net/man3/EVP_PKEY_set1_RSA) should be
used.

# RETURN VALUES

EVP\_PKEY\_new() returns either the newly allocated **EVP\_PKEY** structure or
**NULL** if an error occurred.

EVP\_PKEY\_up\_ref() returns 1 for success and 0 for failure.

# SEE ALSO

[EVP\_PKEY\_set1\_RSA(3)](http://man.he.net/man3/EVP_PKEY_set1_RSA)

# HISTORY

EVP\_PKEY\_new() and EVP\_PKEY\_free() exist in all versions of OpenSSL.

EVP\_PKEY\_up\_ref() was first added to OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
