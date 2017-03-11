# NAME

PKCS12\_parse - parse a PKCS#12 structure

# SYNOPSIS

    #include <openssl/pkcs12.h>

int PKCS12\_parse(PKCS12 \*p12, const char \*pass, EVP\_PKEY \*\*pkey, X509 \*\*cert, STACK\_OF(X509) \*\*ca);

# DESCRIPTION

PKCS12\_parse() parses a PKCS12 structure.

**p12** is the **PKCS12** structure to parse. **pass** is the passphrase to use.
If successful the private key will be written to **\*pkey**, the corresponding
certificate to **\*cert** and any additional certificates to **\*ca**.

# NOTES

The parameters **pkey** and **cert** cannot be **NULL**. **ca** can be <NULL> in
which case additional certificates will be discarded. **\*ca** can also be a
valid STACK in which case additional certificates are appended to **\*ca**. If
**\*ca** is **NULL** a new STACK will be allocated.

The **friendlyName** and **localKeyID** attributes (if present) on each
certificate will be stored in the **alias** and **keyid** attributes of the
**X509** structure.

# RETURN VALUES

PKCS12\_parse() returns 1 for success and zero if an error occurred.

The error can be obtained from [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error)

# BUGS

Only a single private key and corresponding certificate is returned by this
function. More complex PKCS#12 files with multiple private keys will only
return the first match.

Only **friendlyName** and **localKeyID** attributes are currently stored in
certificates. Other attributes are discarded.

Attributes currently cannot be stored in the private key **EVP\_PKEY** structure.

# SEE ALSO

[d2i\_PKCS12(3)](http://man.he.net/man3/d2i_PKCS12)

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
