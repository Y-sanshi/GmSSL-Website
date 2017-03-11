# NAME

DSA\_dup\_DH - create a DH structure out of DSA structure

# SYNOPSIS

    #include <openssl/dsa.h>

    DH * DSA_dup_DH(const DSA *r);

# DESCRIPTION

DSA\_dup\_DH() duplicates DSA parameters/keys as DH parameters/keys. q
is lost during that conversion, but the resulting DH parameters
contain its length.

# RETURN VALUE

DSA\_dup\_DH() returns the new **DH** structure, and NULL on error. The
error codes can be obtained by [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# NOTE

Be careful to avoid small subgroup attacks when using this.

# SEE ALSO

[dh(3)](http://man.he.net/man3/dh), [dsa(3)](http://man.he.net/man3/dsa), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
