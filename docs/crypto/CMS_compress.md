# NAME

CMS\_compress - create a CMS CompressedData structure

# SYNOPSIS

    #include <openssl/cms.h>

    CMS_ContentInfo *CMS_compress(BIO *in, int comp_nid, unsigned int flags);

# DESCRIPTION

CMS\_compress() creates and returns a CMS CompressedData structure. **comp\_nid**
is the compression algorithm to use or **NID\_undef** to use the default
algorithm (zlib compression). **in** is the content to be compressed.
**flags** is an optional set of flags.

# NOTES

The only currently supported compression algorithm is zlib using the NID
NID\_zlib\_compression.

If zlib support is not compiled into OpenSSL then CMS\_compress() will return
an error.

If the **CMS\_TEXT** flag is set MIME headers for type **text/plain** are
prepended to the data.

Normally the supplied content is translated into MIME canonical format (as
required by the S/MIME specifications) if **CMS\_BINARY** is set no translation
occurs. This option should be used if the supplied data is in binary format
otherwise the translation will corrupt it. If **CMS\_BINARY** is set then
**CMS\_TEXT** is ignored.

If the **CMS\_STREAM** flag is set a partial **CMS\_ContentInfo** structure is
returned suitable for streaming I/O: no data is read from the BIO **in**.

The compressed data is included in the CMS\_ContentInfo structure, unless
**CMS\_DETACHED** is set in which case it is omitted. This is rarely used in
practice and is not supported by SMIME\_write\_CMS().

# NOTES

If the flag **CMS\_STREAM** is set the returned **CMS\_ContentInfo** structure is
**not** complete and outputting its contents via a function that does not
properly finalize the **CMS\_ContentInfo** structure will give unpredictable
results.

Several functions including SMIME\_write\_CMS(), i2d\_CMS\_bio\_stream(),
PEM\_write\_bio\_CMS\_stream() finalize the structure. Alternatively finalization
can be performed by obtaining the streaming ASN1 **BIO** directly using
BIO\_new\_CMS().

Additional compression parameters such as the zlib compression level cannot
currently be set.

# RETURN VALUES

CMS\_compress() returns either a CMS\_ContentInfo structure or NULL if an error
occurred. The error can be obtained from ERR\_get\_error(3).

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [CMS\_uncompress(3)](http://man.he.net/man3/CMS_uncompress)

# HISTORY

The **CMS\_STREAM** flag was added in OpenSSL 1.0.0.

# COPYRIGHT

Copyright 2008-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
