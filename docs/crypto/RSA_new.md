# NAME

RSA\_new, RSA\_free - allocate and free RSA objects

# SYNOPSIS

    #include <openssl/rsa.h>

    RSA * RSA_new(void);

    void RSA_free(RSA *rsa);

# DESCRIPTION

RSA\_new() allocates and initializes an **RSA** structure. It is equivalent to
calling RSA\_new\_method(NULL).

RSA\_free() frees the **RSA** structure and its components. The key is
erased before the memory is returned to the system.
If **rsa** is NULL nothing is done.

# RETURN VALUES

If the allocation fails, RSA\_new() returns **NULL** and sets an error
code that can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error). Otherwise it returns
a pointer to the newly allocated structure.

RSA\_free() returns no value.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[RSA\_generate\_key(3)](http://man.he.net/man3/RSA_generate_key),
[RSA\_new\_method(3)](http://man.he.net/man3/RSA_new_method)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
