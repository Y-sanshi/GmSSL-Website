# NAME

DH\_size, DH\_bits - get Diffie-Hellman prime size

# SYNOPSIS

\#include &lt;openssl/dh.h>

int DH\_size(const DH \*dh);

int DH\_bits(const DH \*dh);

# DESCRIPTION

DH\_size() returns the Diffie-Hellman prime size in bytes. It can be used
to determine how much memory must be allocated for the shared secret
computed by DH\_compute\_key().

DH\_bits() returns the number of significant bits.

**dh** and **dh->p** must not be **NULL**.

# RETURN VALUE

The size.

# SEE ALSO

[dh(3)](http://man.he.net/man3/dh), [DH\_generate\_key(3)](http://man.he.net/man3/DH_generate_key),
[BN\_num\_bits(3)](http://man.he.net/man3/BN_num_bits)

# HISTORY

DH\_bits() was added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
