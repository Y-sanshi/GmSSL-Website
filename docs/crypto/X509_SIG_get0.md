# NAME

X509\_SIG\_get0, X509\_SIG\_getm - DigestInfo functions

# SYNOPSIS

    #include <openssl/x509.h>

    void X509_SIG_get0(const X509_SIG *sig, const X509_ALGOR **palg,
                       const ASN1_OCTET_STRING **pdigest);
    void X509_SIG_getm(X509_SIG *sig, X509_ALGOR **palg,
                       ASN1_OCTET_STRING **pdigest,

# DESCRIPTION

X509\_SIG\_get0() returns pointers to the algorithm identifier and digest
value in **sig**. X509\_SIG\_getm() is identical to X509\_SIG\_get0()
except the pointers returned are not constant and can be modified:
for example to initialise them.

# SEE ALSO

[d2i\_X509(3)](http://man.he.net/man3/d2i_X509)

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
