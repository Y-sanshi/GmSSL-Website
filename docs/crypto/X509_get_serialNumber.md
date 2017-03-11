# NAME

X509\_get\_serialNumber,
X509\_get0\_serialNumber,
X509\_set\_serialNumber
\- get or set certificate serial number

# SYNOPSIS

    #include <openssl/x509.h>

    ASN1_INTEGER *X509_get_serialNumber(X509 *x);
    const ASN1_INTEGER *X509_get0_serialNumber(const X509 *x);
    int X509_set_serialNumber(X509 *x, ASN1_INTEGER *serial);

# DESCRIPTION

X509\_get\_serialNumber() returns the serial number of certificate **x** as an
**ASN1\_INTEGER** structure which can be examined or initialised. The value
returned is an internal pointer which **MUST NOT** be freed up after the call.

X509\_get0\_serialNumber() is the same as X509\_get\_serialNumber() except it
accepts a const parameter and returns a const result.

X509\_set\_serialNumber() sets the serial number of certificate **x** to
**serial**. A copy of the serial number is used internally so **serial** should
be freed up after use.

# RETURN VALUES

X509\_get\_serialNumber() and X509\_get0\_serialNumber() return an **ASN1\_INTEGER**
structure.

X509\_set\_serialNumber() returns 1 for success and 0 for failure.

# SEE ALSO

[d2i\_X509(3)](http://man.he.net/man3/d2i_X509),
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[X509\_CRL\_get0\_by\_serial(3)](http://man.he.net/man3/X509_CRL_get0_by_serial),
[X509\_get0\_signature(3)](http://man.he.net/man3/X509_get0_signature),
[X509\_get\_ext\_d2i(3)](http://man.he.net/man3/X509_get_ext_d2i),
[X509\_get\_extension\_flags(3)](http://man.he.net/man3/X509_get_extension_flags),
[X509\_get\_pubkey(3)](http://man.he.net/man3/X509_get_pubkey),
[X509\_get\_subject\_name(3)](http://man.he.net/man3/X509_get_subject_name),
[X509\_NAME\_add\_entry\_by\_txt(3)](http://man.he.net/man3/X509_NAME_add_entry_by_txt),
[X509\_NAME\_ENTRY\_get\_object(3)](http://man.he.net/man3/X509_NAME_ENTRY_get_object),
[X509\_NAME\_get\_index\_by\_NID(3)](http://man.he.net/man3/X509_NAME_get_index_by_NID),
[X509\_NAME\_print\_ex(3)](http://man.he.net/man3/X509_NAME_print_ex),
[X509\_new(3)](http://man.he.net/man3/X509_new),
[X509\_sign(3)](http://man.he.net/man3/X509_sign),
[X509V3\_get\_d2i(3)](http://man.he.net/man3/X509V3_get_d2i),
[X509\_verify\_cert(3)](http://man.he.net/man3/X509_verify_cert)

# HISTORY

X509\_get\_serialNumber() and X509\_set\_serialNumber() are available in
all versions of OpenSSL. X509\_get0\_serialNumber() was added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
