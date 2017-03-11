# NAME

DSA\_SIG\_get0, DSA\_SIG\_set0,
DSA\_SIG\_new, DSA\_SIG\_free - allocate and free DSA signature objects

# SYNOPSIS

    #include <openssl/dsa.h>

    DSA_SIG *DSA_SIG_new(void);
    void DSA_SIG_free(DSA_SIG *a);
    void DSA_SIG_get0(const DSA_SIG *sig, const BIGNUM **pr, const BIGNUM **ps);
    int DSA_SIG_set0(DSA_SIG *sig, BIGNUM *r, BIGNUM *s);

# DESCRIPTION

DSA\_SIG\_new() allocates an empty **DSA\_SIG** structure.

DSA\_SIG\_free() frees the **DSA\_SIG** structure and its components. The
values are erased before the memory is returned to the system.

DSA\_SIG\_get0() returns internal pointers to the **r** and **s** values contained
in **sig**.

The **r** and **s** values can be set by calling DSA\_SIG\_set0() and passing the
new values for **r** and **s** as parameters to the function. Calling this
function transfers the memory management of the values to the DSA\_SIG object,
and therefore the values that have been passed in should not be freed directly
after this function has been called.

# RETURN VALUES

If the allocation fails, DSA\_SIG\_new() returns **NULL** and sets an
error code that can be obtained by
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error). Otherwise it returns a pointer
to the newly allocated structure.

DSA\_SIG\_free() returns no value.

DSA\_SIG\_set0() returns 1 on success or 0 on failure.

# SEE ALSO

[dsa(3)](http://man.he.net/man3/dsa), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[DSA\_do\_sign(3)](http://man.he.net/man3/DSA_do_sign)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
