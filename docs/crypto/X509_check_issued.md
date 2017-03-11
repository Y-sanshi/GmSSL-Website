# NAME

X509\_check\_issued - checks if certificate is issued by another
certificate

# SYNOPSIS

    #include <openssl/x509v3.h>

    int X509_check_issued(X509 *issuer, X509 *subject);

# DESCRIPTION

This function checks if certificate _subject_ was issued using CA
certificate _issuer_. This function takes into account not only
matching of issuer field of _subject_ with subject field of _issuer_,
but also compares **authorityKeyIdentifier** extension of _subject_ with
**subjectKeyIdentifier** of _issuer_ if **authorityKeyIdentifier**
present in the _subject_ certificate and checks **keyUsage** field of
_issuer_.

# RETURN VALUE

Function return **X509\_V\_OK** if certificate _subject_ is issued by
_issuer_ or some **X509\_V\_ERR\*** constant to indicate an error.

# SEE ALSO

[X509\_verify\_cert(3)](http://man.he.net/man3/X509_verify_cert),
[X509\_check\_ca(3)](http://man.he.net/man3/X509_check_ca),
[verify(1)](http://man.he.net/man1/verify)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
