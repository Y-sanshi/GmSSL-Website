# NAME

DH\_new, DH\_free - allocate and free DH objects

# SYNOPSIS

    #include <openssl/dh.h>

    DH* DH_new(void);

    void DH_free(DH *dh);

# DESCRIPTION

DH\_new() allocates and initializes a **DH** structure.

DH\_free() frees the **DH** structure and its components. The values are
erased before the memory is returned to the system.
If **dh** is NULL nothing is done.

# RETURN VALUES

If the allocation fails, DH\_new() returns **NULL** and sets an error
code that can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error). Otherwise it returns
a pointer to the newly allocated structure.

DH\_free() returns no value.

# SEE ALSO

[dh(3)](http://man.he.net/man3/dh), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[DH\_generate\_parameters(3)](http://man.he.net/man3/DH_generate_parameters),
[DH\_generate\_key(3)](http://man.he.net/man3/DH_generate_key)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
