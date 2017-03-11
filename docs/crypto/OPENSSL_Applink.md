# NAME

OPENSSL\_Applink - glue between OpenSSL BIO and Win32 compiler run-time

# SYNOPSIS

    __declspec(dllexport) void **OPENSSL_Applink();

# DESCRIPTION

OPENSSL\_Applink is application-side interface which provides a glue
between OpenSSL BIO layer and Win32 compiler run-time environment.
Even though it appears at application side, it's essentially OpenSSL
private interface. For this reason application developers are not
expected to implement it, but to compile provided module with
compiler of their choice and link it into the target application.
The referred module is available as `applink.c`, located alongside
the public header files (only on the platforms where applicable).

# COPYRIGHT

Copyright 2004-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
