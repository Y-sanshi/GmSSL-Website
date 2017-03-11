# NAME

CMS\_final - finalise a CMS\_ContentInfo structure

# SYNOPSIS

    #include <openssl/cms.h>

    int CMS_final(CMS_ContentInfo *cms, BIO *data, BIO *dcont, unsigned int flags);

# DESCRIPTION

CMS\_final() finalises the structure **cms**. It's purpose is to perform any
operations necessary on **cms** (digest computation for example) and set the
appropriate fields. The parameter **data** contains the content to be
processed. The **dcont** parameter contains a BIO to write content to after
processing: this is only used with detached data and will usually be set to
NULL.

# NOTES

This function will normally be called when the **CMS\_PARTIAL** flag is used. It
should only be used when streaming is not performed because the streaming
I/O functions perform finalisation operations internally.

# RETURN VALUES

CMS\_final() returns 1 for success or 0 for failure.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [CMS\_sign(3)](http://man.he.net/man3/CMS_sign),
[CMS\_encrypt(3)](http://man.he.net/man3/CMS_encrypt)

# COPYRIGHT

Copyright 2008-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
