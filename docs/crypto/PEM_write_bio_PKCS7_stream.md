# NAME

PEM\_write\_bio\_PKCS7\_stream - output PKCS7 structure in PEM format

# SYNOPSIS

    #include <openssl/pkcs7.h>

    int PEM_write_bio_PKCS7_stream(BIO *out, PKCS7 *p7, BIO *data, int flags);

# DESCRIPTION

PEM\_write\_bio\_PKCS7\_stream() outputs a PKCS7 structure in PEM format.

It is otherwise identical to the function SMIME\_write\_PKCS7().

# NOTES

This function is effectively a version of the PEM\_write\_bio\_PKCS7() supporting
streaming.

# RETURN VALUES

PEM\_write\_bio\_PKCS7\_stream() returns 1 for success or 0 for failure.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [PKCS7\_sign(3)](http://man.he.net/man3/PKCS7_sign),
[PKCS7\_verify(3)](http://man.he.net/man3/PKCS7_verify), [PKCS7\_encrypt(3)](http://man.he.net/man3/PKCS7_encrypt)
[PKCS7\_decrypt(3)](http://man.he.net/man3/PKCS7_decrypt),
[SMIME\_write\_PKCS7(3)](http://man.he.net/man3/SMIME_write_PKCS7),
[i2d\_PKCS7\_bio\_stream(3)](http://man.he.net/man3/i2d_PKCS7_bio_stream)

# HISTORY

PEM\_write\_bio\_PKCS7\_stream() was added to OpenSSL 1.0.0

# COPYRIGHT

Copyright 2007-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
