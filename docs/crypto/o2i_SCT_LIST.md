# NAME

o2i\_SCT\_LIST, i2o\_SCT\_LIST, o2i\_SCT, i2o\_SCT -
decode and encode Signed Certificate Timestamp lists in TLS wire format

# SYNOPSIS

    #include <openssl/ct.h>

    STACK_OF(SCT) *o2i_SCT_LIST(STACK_OF(SCT) **a, const unsigned char **pp, size_t len);
    int i2o_SCT_LIST(const STACK_OF(SCT) *a, unsigned char **pp);
    SCT *o2i_SCT(SCT **psct, const unsigned char **in, size_t len);
    int i2o_SCT(const SCT *sct, unsigned char **out);

# DESCRIPTION

The SCT\_LIST and SCT functions are very similar to the i2d and d2i family of
functions, except that they convert to and from TLS wire format, as described in
RFC 6962. See [d2i\_SCT\_LIST](https://metacpan.org/pod/d2i_SCT_LIST) for more information about how the parameters are
treated and the return values.

# RETURN VALUES

All of the functions have return values consistent with those stated for
[d2i\_SCT\_LIST](https://metacpan.org/pod/d2i_SCT_LIST) and [i2d\_SCT\_LIST](https://metacpan.org/pod/i2d_SCT_LIST).

# SEE ALSO

[ct(3)](http://man.he.net/man3/ct),
[d2i\_SCT\_LIST(3)](http://man.he.net/man3/d2i_SCT_LIST),
[i2d\_SCT\_LIST(3)](http://man.he.net/man3/i2d_SCT_LIST)

# HISTORY

These functions were added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
