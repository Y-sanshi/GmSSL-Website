# NAME

DSA\_set\_default\_method, DSA\_get\_default\_method,
DSA\_set\_method, DSA\_new\_method, DSA\_OpenSSL - select DSA method

# SYNOPSIS

    #include <openssl/dsa.h>

    void DSA_set_default_method(const DSA_METHOD *meth);

    const DSA_METHOD *DSA_get_default_method(void);

    int DSA_set_method(DSA *dsa, const DSA_METHOD *meth);

    DSA *DSA_new_method(ENGINE *engine);

    DSA_METHOD *DSA_OpenSSL(void);

# DESCRIPTION

A **DSA\_METHOD** specifies the functions that OpenSSL uses for DSA
operations. By modifying the method, alternative implementations
such as hardware accelerators may be used. IMPORTANT: See the NOTES section for
important information about how these DSA API functions are affected by the use
of **ENGINE** API calls.

Initially, the default DSA\_METHOD is the OpenSSL internal implementation,
as returned by DSA\_OpenSSL().

DSA\_set\_default\_method() makes **meth** the default method for all DSA
structures created later. **NB**: This is true only whilst no ENGINE has
been set as a default for DSA, so this function is no longer recommended.

DSA\_get\_default\_method() returns a pointer to the current default
DSA\_METHOD. However, the meaningfulness of this result is dependent on
whether the ENGINE API is being used, so this function is no longer
recommended.

DSA\_set\_method() selects **meth** to perform all operations using the key
**rsa**. This will replace the DSA\_METHOD used by the DSA key and if the
previous method was supplied by an ENGINE, the handle to that ENGINE will
be released during the change. It is possible to have DSA keys that only
work with certain DSA\_METHOD implementations (eg. from an ENGINE module
that supports embedded hardware-protected keys), and in such cases
attempting to change the DSA\_METHOD for the key can have unexpected
results. See [DSA\_meth\_new](https://metacpan.org/pod/DSA_meth_new) for information on constructing custom DSA\_METHOD
objects;

DSA\_new\_method() allocates and initializes a DSA structure so that **engine**
will be used for the DSA operations. If **engine** is NULL, the default engine
for DSA operations is used, and if no default ENGINE is set, the DSA\_METHOD
controlled by DSA\_set\_default\_method() is used.

# RETURN VALUES

DSA\_OpenSSL() and DSA\_get\_default\_method() return pointers to the respective
**DSA\_METHOD**s.

DSA\_set\_default\_method() returns no value.

DSA\_set\_method() returns non-zero if the provided **meth** was successfully set as
the method for **dsa** (including unloading the ENGINE handle if the previous
method was supplied by an ENGINE).

DSA\_new\_method() returns NULL and sets an error code that can be
obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error) if the allocation
fails. Otherwise it returns a pointer to the newly allocated structure.

# SEE ALSO

[dsa(3)](http://man.he.net/man3/dsa), [DSA\_new(3)](http://man.he.net/man3/DSA_new), [DSA\_meth\_new(3)](http://man.he.net/man3/DSA_meth_new)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
