# NAME

SMIME\_read\_CMS - parse S/MIME message

# SYNOPSIS

    #include <openssl/cms.h>

    CMS_ContentInfo *SMIME_read_CMS(BIO *in, BIO **bcont);

# DESCRIPTION

SMIME\_read\_CMS() parses a message in S/MIME format.

**in** is a BIO to read the message from.

If cleartext signing is used then the content is saved in a memory bio which is
written to **\*bcont**, otherwise **\*bcont** is set to NULL.

The parsed CMS\_ContentInfo structure is returned or NULL if an
error occurred.

# NOTES

If **\*bcont** is not NULL then the message is clear text signed. **\*bcont** can
then be passed to CMS\_verify() with the **CMS\_DETACHED** flag set.

Otherwise the type of the returned structure can be determined
using CMS\_get0\_type().

To support future functionality if **bcont** is not NULL **\*bcont** should be
initialized to NULL. For example:

    BIO *cont = NULL;
    CMS_ContentInfo *cms;

    cms = SMIME_read_CMS(in, &cont);

# BUGS

The MIME parser used by SMIME\_read\_CMS() is somewhat primitive.  While it will
handle most S/MIME messages more complex compound formats may not work.

The parser assumes that the CMS\_ContentInfo structure is always base64 encoded
and will not handle the case where it is in binary format or uses quoted
printable format.

The use of a memory BIO to hold the signed content limits the size of message
which can be processed due to memory restraints: a streaming single pass option
should be available.

# RETURN VALUES

SMIME\_read\_CMS() returns a valid **CMS\_ContentInfo** structure or **NULL**
if an error occurred. The error can be obtained from ERR\_get\_error(3).

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [CMS\_type(3)](http://man.he.net/man3/CMS_type)
[SMIME\_read\_CMS(3)](http://man.he.net/man3/SMIME_read_CMS), [CMS\_sign(3)](http://man.he.net/man3/CMS_sign),
[CMS\_verify(3)](http://man.he.net/man3/CMS_verify), [CMS\_encrypt(3)](http://man.he.net/man3/CMS_encrypt)
[CMS\_decrypt(3)](http://man.he.net/man3/CMS_decrypt)

# COPYRIGHT

Copyright 2008-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
