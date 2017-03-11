# NAME

EVP\_PKEY\_set1\_RSA, EVP\_PKEY\_set1\_DSA, EVP\_PKEY\_set1\_DH, EVP\_PKEY\_set1\_EC\_KEY,
EVP\_PKEY\_get1\_RSA, EVP\_PKEY\_get1\_DSA, EVP\_PKEY\_get1\_DH, EVP\_PKEY\_get1\_EC\_KEY,
EVP\_PKEY\_get0\_RSA, EVP\_PKEY\_get0\_DSA, EVP\_PKEY\_get0\_DH, EVP\_PKEY\_get0\_EC\_KEY,
EVP\_PKEY\_assign\_RSA, EVP\_PKEY\_assign\_DSA, EVP\_PKEY\_assign\_DH, EVP\_PKEY\_assign\_EC\_KEY,
EVP\_PKEY\_get0\_hmac,
EVP\_PKEY\_type, EVP\_PKEY\_id, EVP\_PKEY\_base\_id
\- EVP\_PKEY assignment functions

# SYNOPSIS

    #include <openssl/evp.h>

    int EVP_PKEY_set1_RSA(EVP_PKEY *pkey, RSA *key);
    int EVP_PKEY_set1_DSA(EVP_PKEY *pkey, DSA *key);
    int EVP_PKEY_set1_DH(EVP_PKEY *pkey, DH *key);
    int EVP_PKEY_set1_EC_KEY(EVP_PKEY *pkey, EC_KEY *key);

    RSA *EVP_PKEY_get1_RSA(EVP_PKEY *pkey);
    DSA *EVP_PKEY_get1_DSA(EVP_PKEY *pkey);
    DH *EVP_PKEY_get1_DH(EVP_PKEY *pkey);
    EC_KEY *EVP_PKEY_get1_EC_KEY(EVP_PKEY *pkey);

    const unsigned char *EVP_PKEY_get0_hmac(const EVP_PKEY *pkey, size_t *len);
    RSA *EVP_PKEY_get0_RSA(EVP_PKEY *pkey);
    DSA *EVP_PKEY_get0_DSA(EVP_PKEY *pkey);
    DH *EVP_PKEY_get0_DH(EVP_PKEY *pkey);
    EC_KEY *EVP_PKEY_get0_EC_KEY(EVP_PKEY *pkey);

    int EVP_PKEY_assign_RSA(EVP_PKEY *pkey, RSA *key);
    int EVP_PKEY_assign_DSA(EVP_PKEY *pkey, DSA *key);
    int EVP_PKEY_assign_DH(EVP_PKEY *pkey, DH *key);
    int EVP_PKEY_assign_EC_KEY(EVP_PKEY *pkey, EC_KEY *key);

    int EVP_PKEY_id(const EVP_PKEY *pkey);
    int EVP_PKEY_base_id(const EVP_PKEY *pkey);
    int EVP_PKEY_type(int type);

# DESCRIPTION

EVP\_PKEY\_set1\_RSA(), EVP\_PKEY\_set1\_DSA(), EVP\_PKEY\_set1\_DH() and
EVP\_PKEY\_set1\_EC\_KEY() set the key referenced by **pkey** to **key**.

EVP\_PKEY\_get1\_RSA(), EVP\_PKEY\_get1\_DSA(), EVP\_PKEY\_get1\_DH() and
EVP\_PKEY\_get1\_EC\_KEY() return the referenced key in **pkey** or
**NULL** if the key is not of the correct type.

EVP\_PKEY\_get0\_hmac(), EVP\_PKEY\_get0\_RSA(), EVP\_PKEY\_get0\_DSA(),
EVP\_PKEY\_get0\_DH() and EVP\_PKEY\_get0\_EC\_KEY() also return the
referenced key in **pkey** or **NULL** if the key is not of the
correct type but the reference count of the returned key is
**not** incremented and so must not be freed up after use.

EVP\_PKEY\_assign\_RSA(), EVP\_PKEY\_assign\_DSA(), EVP\_PKEY\_assign\_DH()
and EVP\_PKEY\_assign\_EC\_KEY() also set the referenced key to **key**
however these use the supplied **key** internally and so **key**
will be freed when the parent **pkey** is freed.

EVP\_PKEY\_base\_id() returns the type of **pkey**. For example
an RSA key will return **EVP\_PKEY\_RSA**.

EVP\_PKEY\_id() returns the actual OID associated with **pkey**. Historically keys
using the same algorithm could use different OIDs. For example an RSA key could
use the OIDs corresponding to the NIDs **NID\_rsaEncryption** (equivalent to
**EVP\_PKEY\_RSA**) or **NID\_rsa** (equivalent to **EVP\_PKEY\_RSA2**). The use of
alternative non-standard OIDs is now rare so **EVP\_PKEY\_RSA2** et al are not
often seen in practice.

EVP\_PKEY\_type() returns the underlying type of the NID **type**. For example
EVP\_PKEY\_type(EVP\_PKEY\_RSA2) will return **EVP\_PKEY\_RSA**.

# NOTES

In accordance with the OpenSSL naming convention the key obtained
from or assigned to the **pkey** using the **1** functions must be
freed as well as **pkey**.

EVP\_PKEY\_assign\_RSA(), EVP\_PKEY\_assign\_DSA(), EVP\_PKEY\_assign\_DH()
and EVP\_PKEY\_assign\_EC\_KEY() are implemented as macros.

Most applications wishing to know a key type will simply call
EVP\_PKEY\_base\_id() and will not care about the actual type:
which will be identical in almost all cases.

Previous versions of this document suggested using EVP\_PKEY\_type(pkey->type)
to determine the type of a key. Since **EVP\_PKEY** is now opaque this
is no longer possible: the equivalent is EVP\_PKEY\_base\_id(pkey).

# RETURN VALUES

EVP\_PKEY\_set1\_RSA(), EVP\_PKEY\_set1\_DSA(), EVP\_PKEY\_set1\_DH() and
EVP\_PKEY\_set1\_EC\_KEY() return 1 for success or 0 for failure.

EVP\_PKEY\_get1\_RSA(), EVP\_PKEY\_get1\_DSA(), EVP\_PKEY\_get1\_DH() and
EVP\_PKEY\_get1\_EC\_KEY() return the referenced key or **NULL** if
an error occurred.

EVP\_PKEY\_assign\_RSA(), EVP\_PKEY\_assign\_DSA(), EVP\_PKEY\_assign\_DH()
and EVP\_PKEY\_assign\_EC\_KEY() return 1 for success and 0 for failure.

EVP\_PKEY\_base\_id(), EVP\_PKEY\_id() and EVP\_PKEY\_type() return a key
type or **NID\_undef** (equivalently **EVP\_PKEY\_NONE**) on error.

# SEE ALSO

[EVP\_PKEY\_new(3)](http://man.he.net/man3/EVP_PKEY_new)

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
