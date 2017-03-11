# NAME

CMS\_uncompress - uncompress a CMS CompressedData structure

# SYNOPSIS

    #include <openssl/cms.h>

    int CMS_uncompress(CMS_ContentInfo *cms, BIO *dcont, BIO *out, unsigned int flags);

# DESCRIPTION

CMS\_uncompress() extracts and uncompresses the content from a CMS
CompressedData structure **cms**. **data** is a BIO to write the content to and
**flags** is an optional set of flags.

The **dcont** parameter is used in the rare case where the compressed content
is detached. It will normally be set to NULL.

# NOTES

The only currently supported compression algorithm is zlib: if the structure
indicates the use of any other algorithm an error is returned.

If zlib support is not compiled into OpenSSL then CMS\_uncompress() will always
return an error.

The following flags can be passed in the **flags** parameter.

If the **CMS\_TEXT** flag is set MIME headers for type **text/plain** are deleted
from the content. If the content is not of type **text/plain** then an error is
returned.

# RETURN VALUES

CMS\_uncompress() returns either 1 for success or 0 for failure. The error can
be obtained from ERR\_get\_error(3)

# BUGS

The lack of single pass processing and the need to hold all data in memory as
mentioned in CMS\_verify() also applies to CMS\_decompress().

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [CMS\_compress(3)](http://man.he.net/man3/CMS_compress)

# COPYRIGHT

Copyright 2008-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
