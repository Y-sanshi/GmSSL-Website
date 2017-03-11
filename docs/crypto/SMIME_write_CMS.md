# NAME

SMIME\_write\_CMS - convert CMS structure to S/MIME format

# SYNOPSIS

    #include <openssl/cms.h>

    int SMIME_write_CMS(BIO *out, CMS_ContentInfo *cms, BIO *data, int flags);

# DESCRIPTION

SMIME\_write\_CMS() adds the appropriate MIME headers to a CMS
structure to produce an S/MIME message.

**out** is the BIO to write the data to. **cms** is the appropriate
**CMS\_ContentInfo** structure. If streaming is enabled then the content must be
supplied in the **data** argument. **flags** is an optional set of flags.

# NOTES

The following flags can be passed in the **flags** parameter.

If **CMS\_DETACHED** is set then cleartext signing will be used, this option only
makes sense for SignedData where **CMS\_DETACHED** is also set when CMS\_sign() is
called.

If the **CMS\_TEXT** flag is set MIME headers for type **text/plain** are added to
the content, this only makes sense if **CMS\_DETACHED** is also set.

If the **CMS\_STREAM** flag is set streaming is performed. This flag should only
be set if **CMS\_STREAM** was also set in the previous call to a CMS\_ContentInfo
creation function.

If cleartext signing is being used and **CMS\_STREAM** not set then the data must
be read twice: once to compute the signature in CMS\_sign() and once to output
the S/MIME message.

If streaming is performed the content is output in BER format using indefinite
length constructed encoding except in the case of signed data with detached
content where the content is absent and DER format is used.

# BUGS

SMIME\_write\_CMS() always base64 encodes CMS structures, there should be an
option to disable this.

# RETURN VALUES

SMIME\_write\_CMS() returns 1 for success or 0 for failure.

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [CMS\_sign(3)](http://man.he.net/man3/CMS_sign),
[CMS\_verify(3)](http://man.he.net/man3/CMS_verify), [CMS\_encrypt(3)](http://man.he.net/man3/CMS_encrypt)
[CMS\_decrypt(3)](http://man.he.net/man3/CMS_decrypt)

# COPYRIGHT

Copyright 2008-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
