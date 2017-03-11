# NAME

X509\_verify\_cert - discover and verify X509 certificate chain

# SYNOPSIS

    #include <openssl/x509.h>

    int X509_verify_cert(X509_STORE_CTX *ctx);

# DESCRIPTION

The X509\_verify\_cert() function attempts to discover and validate a
certificate chain based on parameters in **ctx**. A complete description of
the process is contained in the [verify(1)](http://man.he.net/man1/verify) manual page.

# RETURN VALUES

If a complete chain can be built and validated this function returns 1,
otherwise it return zero, in exceptional circumstances it can also
return a negative code.

If the function fails additional error information can be obtained by
examining **ctx** using, for example X509\_STORE\_CTX\_get\_error().

# NOTES

Applications rarely call this function directly but it is used by
OpenSSL internally for certificate validation, in both the S/MIME and
SSL/TLS code.

A negative return value from X509\_verify\_cert() can occur if it is invoked
incorrectly, such as with no certificate set in **ctx**, or when it is called
twice in succession without reinitialising **ctx** for the second call.
A negative return value can also happen due to internal resource problems or if
a retry operation is requested during internal lookups (which never happens
with standard lookup methods).
Applications must check for <= 0 return value on error.

# BUGS

This function uses the header **x509.h** as opposed to most chain verification
functions which use **x509\_vfy.h**.

# SEE ALSO

[X509\_STORE\_CTX\_get\_error(3)](http://man.he.net/man3/X509_STORE_CTX_get_error)

# COPYRIGHT

Copyright 2009-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
