# NAME

SSL\_state\_string, SSL\_state\_string\_long - get textual description of state of an SSL object

# SYNOPSIS

    #include <openssl/ssl.h>

    const char *SSL_state_string(const SSL *ssl);
    const char *SSL_state_string_long(const SSL *ssl);

# DESCRIPTION

SSL\_state\_string() returns a 6 letter string indicating the current state
of the SSL object **ssl**.

SSL\_state\_string\_long() returns a string indicating the current state of
the SSL object **ssl**.

# NOTES

During its use, an SSL objects passes several states. The state is internally
maintained. Querying the state information is not very informative before
or when a connection has been established. It however can be of significant
interest during the handshake.

When using non-blocking sockets, the function call performing the handshake
may return with SSL\_ERROR\_WANT\_READ or SSL\_ERROR\_WANT\_WRITE condition,
so that SSL\_state\_string\[\_long\]() may be called.

For both blocking or non-blocking sockets, the details state information
can be used within the info\_callback function set with the
SSL\_set\_info\_callback() call.

# RETURN VALUES

Detailed description of possible states to be included later.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_CTX\_set\_info\_callback(3)](http://man.he.net/man3/SSL_CTX_set_info_callback)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
