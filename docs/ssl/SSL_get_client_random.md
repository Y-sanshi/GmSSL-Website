# NAME

SSL\_get\_client\_random, SSL\_get\_server\_random, SSL\_SESSION\_get\_master\_key - retrieve internal TLS/SSL random values and master key

# SYNOPSIS

    #include <openssl/ssl.h>

    size_t SSL_get_client_random(const SSL *ssl, unsigned char *out, size_t outlen);
    size_t SSL_get_server_random(const SSL *ssl, unsigned char *out, size_t outlen);
    size_t SSL_SESSION_get_master_key(const SSL_SESSION *session, unsigned char *out, size_t outlen);

# DESCRIPTION

SSL\_get\_client\_random() extracts the random value sent from the client
to the server during the initial SSL/TLS handshake.  It copies as many
bytes as it can of this value into the buffer provided in **out**,
which must have at least **outlen** bytes available. It returns the
total number of bytes that were actually copied.  If **outlen** is
zero, SSL\_get\_client\_random() copies nothing, and returns the
total size of the client\_random value.

SSL\_get\_server\_random() behaves the same, but extracts the random value
sent from the server to the client during the initial SSL/TLS handshake.

SSL\_SESSION\_get\_master\_key() behaves the same, but extracts the master
secret used to guarantee the security of the SSL/TLS session.  This one
can be dangerous if misused; see NOTES below.

# NOTES

You probably shouldn't use these functions.

These functions expose internal values from the TLS handshake, for
use in low-level protocols.  You probably should not use them, unless
you are implementing something that needs access to the internal protocol
details.

Despite the names of SSL\_get\_client\_random() and SSL\_get\_server\_random(), they
ARE NOT random number generators.  Instead, they return the mostly-random values that
were already generated and used in the TLS protocol.  Using them
in place of RAND\_bytes() would be grossly foolish.

The security of your TLS session depends on keeping the master key secret:
do not expose it, or any information about it, to anybody.
If you need to calculate another secret value that depends on the master
secret, you should probably use SSL\_export\_keying\_material() instead, and
forget that you ever saw these functions.

In current versions of the TLS protocols, the length of client\_random
(and also server\_random) is always SSL3\_RANDOM\_SIZE bytes. Support for
other outlen arguments to the SSL\_get\_\*\_random() functions is provided
in case of the unlikely event that a future version or variant of TLS
uses some other length there.

Finally, though the "client\_random" and "server\_random" values are called
"random", many TLS implementations will generate four bytes of those
values based on their view of the current time.

# RETURN VALUES

If **outlen** is greater than 0, these functions return the number of bytes
actually copied, which will be less than or equal to **outlen**.

If **outlen** is 0, these functions return the maximum number
of bytes they would copy--that is, the length of the underlying field.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[RAND\_bytes(3)](http://man.he.net/man3/RAND_bytes),
[SSL\_export\_keying\_material(3)](http://man.he.net/man3/SSL_export_keying_material)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
