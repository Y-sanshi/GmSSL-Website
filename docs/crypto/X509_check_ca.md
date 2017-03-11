# NAME

X509\_check\_ca - check if given certificate is CA certificate

# SYNOPSIS

    #include <openssl/x509v3.h>

    int X509_check_ca(X509 *cert);

# DESCRIPTION

This function checks if given certificate is CA certificate (can be used
to sign other certificates).

# RETURN VALUE

Function return 0, if it is not CA certificate, 1 if it is proper X509v3
CA certificate with **basicConstraints** extension CA:TRUE,
3, if it is self-signed X509 v1 certificate, 4, if it is certificate with
**keyUsage** extension with bit **keyCertSign** set, but without
**basicConstraints**, and 5 if it has outdated Netscape Certificate Type
extension telling that it is CA certificate.

Actually, any non-zero value means that this certificate could have been
used to sign other certificates.

# SEE ALSO

[X509\_verify\_cert(3)](http://man.he.net/man3/X509_verify_cert),
[X509\_check\_issued(3)](http://man.he.net/man3/X509_check_issued),
[X509\_check\_purpose(3)](http://man.he.net/man3/X509_check_purpose)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
