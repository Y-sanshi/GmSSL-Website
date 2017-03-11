# NAME

DSA\_new, DSA\_free - allocate and free DSA objects

# SYNOPSIS

    #include <openssl/dsa.h>

    DSA* DSA_new(void);

    void DSA_free(DSA *dsa);

# DESCRIPTION

DSA\_new() allocates and initializes a **DSA** structure. It is equivalent to
calling DSA\_new\_method(NULL).

DSA\_free() frees the **DSA** structure and its components. The values are
erased before the memory is returned to the system.
If **dsa** is NULL nothing is done.

# RETURN VALUES

If the allocation fails, DSA\_new() returns **NULL** and sets an error
code that can be obtained by
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error). Otherwise it returns a pointer
to the newly allocated structure.

DSA\_free() returns no value.

# SEE ALSO

[dsa(3)](http://man.he.net/man3/dsa), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[DSA\_generate\_parameters(3)](http://man.he.net/man3/DSA_generate_parameters),
[DSA\_generate\_key(3)](http://man.he.net/man3/DSA_generate_key)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
