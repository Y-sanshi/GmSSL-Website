# NAME

i2d\_CMS\_bio\_stream - output CMS\_ContentInfo structure in BER format

# SYNOPSIS

    #include <openssl/cms.h>

    int i2d_CMS_bio_stream(BIO *out, CMS_ContentInfo *cms, BIO *data, int flags);

# DESCRIPTION

i2d\_CMS\_bio\_stream() outputs a CMS\_ContentInfo structure in BER format.

It is otherwise identical to the function SMIME\_write\_CMS().

# NOTES

This function is effectively a version of the i2d\_CMS\_bio() supporting
streaming.

# BUGS

The prefix "i2d" is arguably wrong because the function outputs BER format.

# RETURN VALUES

i2d\_CMS\_bio\_stream() returns 1 for success or 0 for failure.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [CMS\_sign(3)](http://man.he.net/man3/CMS_sign),
[CMS\_verify(3)](http://man.he.net/man3/CMS_verify), [CMS\_encrypt(3)](http://man.he.net/man3/CMS_encrypt)
[CMS\_decrypt(3)](http://man.he.net/man3/CMS_decrypt),
[SMIME\_write\_CMS(3)](http://man.he.net/man3/SMIME_write_CMS),
[PEM\_write\_bio\_CMS\_stream(3)](http://man.he.net/man3/PEM_write_bio_CMS_stream)

# HISTORY

i2d\_CMS\_bio\_stream() was added to OpenSSL 1.0.0

# COPYRIGHT

Copyright 2008-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
