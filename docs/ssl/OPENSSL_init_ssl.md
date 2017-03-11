# NAME

OPENSSL\_init\_ssl - OpenSSL (libssl and libcrypto) initialisation

# SYNOPSIS

    #include <openssl/ssl.h>

    int OPENSSL_init_ssl(uint64_t opts, const OPENSSL_INIT_SETTINGS *settings);

# DESCRIPTION

During normal operation OpenSSL (libssl and libcrypto) will allocate various
resources at start up that must, subsequently, be freed on close down of the
library. Additionally some resources are allocated on a per thread basis (if the
application is multi-threaded), and these resources must be freed prior to the
thread closing.

As of version 1.1.0 OpenSSL will automatically allocate all resources that it
needs so no explicit initialisation is required. Similarly it will also
automatically deinitialise as required.

However, there may be situations when explicit initialisation is desirable or
needed, for example when some non-default initialisation is required. The
function OPENSSL\_init\_ssl() can be used for this purpose. Calling
this function will explicitly initialise BOTH libcrypto and libssl. To
explicitly initialise ONLY libcrypto see the
[OPENSSL\_init\_crypto(3)](http://man.he.net/man3/OPENSSL_init_crypto) function.

Numerous internal OpenSSL functions call OPENSSL\_init\_ssl().
Therefore, in order to perform non-default initialisation,
OPENSSL\_init\_ssl() MUST be called by application code prior to
any other OpenSSL function calls.

The **opts** parameter specifies which aspects of libssl and libcrypto should be
initialised. Valid options for libcrypto are described on the
[OPENSSL\_init\_crypto(3)](http://man.he.net/man3/OPENSSL_init_crypto) page. In addition to any libcrypto
specific option the following libssl options can also be used:

- OPENSSL\_INIT\_NO\_LOAD\_SSL\_STRINGS

    Suppress automatic loading of the libssl error strings. This option is
    not a default option. Once selected subsequent calls to
    OPENSSL\_init\_ssl() with the option
    **OPENSSL\_INIT\_LOAD\_SSL\_STRINGS** will be ignored.

- OPENSSL\_INIT\_LOAD\_SSL\_STRINGS

    Automatic loading of the libssl error strings. This option is a
    default option. Once selected subsequent calls to
    OPENSSL\_init\_ssl() with the option
    **OPENSSL\_INIT\_LOAD\_SSL\_STRINGS** will be ignored.

OPENSSL\_init\_ssl() takes a **settings** parameter which can be used to
set parameter values.  See [OPENSSL\_init\_crypto(3)](http://man.he.net/man3/OPENSSL_init_crypto) for details.

# RETURN VALUES

The function OPENSSL\_init\_ssl() returns 1 on success or 0 on error.

# SEE ALSO

[OPENSSL\_init\_crypto(3)](http://man.he.net/man3/OPENSSL_init_crypto)

# HISTORY

The OPENSSL\_init\_ssl() function was added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
