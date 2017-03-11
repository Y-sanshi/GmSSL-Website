# NAME

DSA\_do\_sign, DSA\_do\_verify - raw DSA signature operations

# SYNOPSIS

    #include <openssl/dsa.h>

    DSA_SIG *DSA_do_sign(const unsigned char *dgst, int dlen, DSA *dsa);

    int DSA_do_verify(const unsigned char *dgst, int dgst_len,
                DSA_SIG *sig, DSA *dsa);

# DESCRIPTION

DSA\_do\_sign() computes a digital signature on the **len** byte message
digest **dgst** using the private key **dsa** and returns it in a
newly allocated **DSA\_SIG** structure.

[DSA\_sign\_setup(3)](http://man.he.net/man3/DSA_sign_setup) may be used to precompute part
of the signing operation in case signature generation is
time-critical.

DSA\_do\_verify() verifies that the signature **sig** matches a given
message digest **dgst** of size **len**.  **dsa** is the signer's public
key.

# RETURN VALUES

DSA\_do\_sign() returns the signature, NULL on error.  DSA\_do\_verify()
returns 1 for a valid signature, 0 for an incorrect signature and -1
on error. The error codes can be obtained by
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# SEE ALSO

[dsa(3)](http://man.he.net/man3/dsa), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [rand(3)](http://man.he.net/man3/rand),
[DSA\_SIG\_new(3)](http://man.he.net/man3/DSA_SIG_new),
[DSA\_sign(3)](http://man.he.net/man3/DSA_sign)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
