# NAME

RSA\_size, RSA\_bits - get RSA modulus size

# SYNOPSIS

\#include &lt;openssl/rsa.h>

int RSA\_size(const RSA \*rsa);

int RSA\_bits(const RSA \*rsa);

# DESCRIPTION

RSA\_size() returns the RSA modulus size in bytes. It can be used to
determine how much memory must be allocated for an RSA encrypted
value.

RSA\_bits() returns the number of significant bits.

**rsa** and **rsa->n** must not be **NULL**.

# RETURN VALUE

The size.

# SEE ALSO

[BN\_num\_bits(3)](http://man.he.net/man3/BN_num_bits)

# HISTORY

RSA\_bits() was added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
