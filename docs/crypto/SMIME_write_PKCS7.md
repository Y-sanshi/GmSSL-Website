# NAME

SMIME\_write\_PKCS7 - convert PKCS#7 structure to S/MIME format

# SYNOPSIS

    #include <openssl/pkcs7.h>

    int SMIME_write_PKCS7(BIO *out, PKCS7 *p7, BIO *data, int flags);

# DESCRIPTION

SMIME\_write\_PKCS7() adds the appropriate MIME headers to a PKCS#7
structure to produce an S/MIME message.

**out** is the BIO to write the data to. **p7** is the appropriate **PKCS7**
structure. If streaming is enabled then the content must be supplied in the
**data** argument. **flags** is an optional set of flags.

# NOTES

The following flags can be passed in the **flags** parameter.

If **PKCS7\_DETACHED** is set then cleartext signing will be used,
this option only makes sense for signedData where **PKCS7\_DETACHED**
is also set when PKCS7\_sign() is also called.

If the **PKCS7\_TEXT** flag is set MIME headers for type **text/plain**
are added to the content, this only makes sense if **PKCS7\_DETACHED**
is also set.

If the **PKCS7\_STREAM** flag is set streaming is performed. This flag should
only be set if **PKCS7\_STREAM** was also set in the previous call to
PKCS7\_sign() or PKCS7\_encrypt().

If cleartext signing is being used and **PKCS7\_STREAM** not set then
the data must be read twice: once to compute the signature in PKCS7\_sign()
and once to output the S/MIME message.

If streaming is performed the content is output in BER format using indefinite
length constructed encoding except in the case of signed data with detached
content where the content is absent and DER format is used.

# BUGS

SMIME\_write\_PKCS7() always base64 encodes PKCS#7 structures, there
should be an option to disable this.

# RETURN VALUES

SMIME\_write\_PKCS7() returns 1 for success or 0 for failure.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [PKCS7\_sign(3)](http://man.he.net/man3/PKCS7_sign),
[PKCS7\_verify(3)](http://man.he.net/man3/PKCS7_verify), [PKCS7\_encrypt(3)](http://man.he.net/man3/PKCS7_encrypt)
[PKCS7\_decrypt(3)](http://man.he.net/man3/PKCS7_decrypt)

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
