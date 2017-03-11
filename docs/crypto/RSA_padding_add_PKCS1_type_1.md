# NAME

RSA\_padding\_add\_PKCS1\_type\_1, RSA\_padding\_check\_PKCS1\_type\_1,
RSA\_padding\_add\_PKCS1\_type\_2, RSA\_padding\_check\_PKCS1\_type\_2,
RSA\_padding\_add\_PKCS1\_OAEP, RSA\_padding\_check\_PKCS1\_OAEP,
RSA\_padding\_add\_SSLv23, RSA\_padding\_check\_SSLv23,
RSA\_padding\_add\_none, RSA\_padding\_check\_none - asymmetric encryption
padding

# SYNOPSIS

    #include <openssl/rsa.h>

    int RSA_padding_add_PKCS1_type_1(unsigned char *to, int tlen,
       unsigned char *f, int fl);

    int RSA_padding_check_PKCS1_type_1(unsigned char *to, int tlen,
       unsigned char *f, int fl, int rsa_len);

    int RSA_padding_add_PKCS1_type_2(unsigned char *to, int tlen,
       unsigned char *f, int fl);

    int RSA_padding_check_PKCS1_type_2(unsigned char *to, int tlen,
       unsigned char *f, int fl, int rsa_len);

    int RSA_padding_add_PKCS1_OAEP(unsigned char *to, int tlen,
       unsigned char *f, int fl, unsigned char *p, int pl);

    int RSA_padding_check_PKCS1_OAEP(unsigned char *to, int tlen,
       unsigned char *f, int fl, int rsa_len, unsigned char *p, int pl);

    int RSA_padding_add_SSLv23(unsigned char *to, int tlen,
       unsigned char *f, int fl);

    int RSA_padding_check_SSLv23(unsigned char *to, int tlen,
       unsigned char *f, int fl, int rsa_len);

    int RSA_padding_add_none(unsigned char *to, int tlen,
       unsigned char *f, int fl);

    int RSA_padding_check_none(unsigned char *to, int tlen,
       unsigned char *f, int fl, int rsa_len);

# DESCRIPTION

The RSA\_padding\_xxx\_xxx() functions are called from the RSA encrypt,
decrypt, sign and verify functions. Normally they should not be called
from application programs.

However, they can also be called directly to implement padding for other
asymmetric ciphers. RSA\_padding\_add\_PKCS1\_OAEP() and
RSA\_padding\_check\_PKCS1\_OAEP() may be used in an application combined
with **RSA\_NO\_PADDING** in order to implement OAEP with an encoding
parameter.

RSA\_padding\_add\_xxx() encodes **fl** bytes from **f** so as to fit into
**tlen** bytes and stores the result at **to**. An error occurs if **fl**
does not meet the size requirements of the encoding method.

The following encoding methods are implemented:

- PKCS1\_type\_1

    PKCS #1 v2.0 EMSA-PKCS1-v1\_5 (PKCS #1 v1.5 block type 1); used for signatures

- PKCS1\_type\_2

    PKCS #1 v2.0 EME-PKCS1-v1\_5 (PKCS #1 v1.5 block type 2)

- PKCS1\_OAEP

    PKCS #1 v2.0 EME-OAEP

- SSLv23

    PKCS #1 EME-PKCS1-v1\_5 with SSL-specific modification

- none

    simply copy the data

The random number generator must be seeded prior to calling
RSA\_padding\_add\_xxx().

RSA\_padding\_check\_xxx() verifies that the **fl** bytes at **f** contain
a valid encoding for a **rsa\_len** byte RSA key in the respective
encoding method and stores the recovered data of at most **tlen** bytes
(for **RSA\_NO\_PADDING**: of size **tlen**)
at **to**.

For RSA\_padding\_xxx\_OAEP(), **p** points to the encoding parameter
of length **pl**. **p** may be **NULL** if **pl** is 0.

# RETURN VALUES

The RSA\_padding\_add\_xxx() functions return 1 on success, 0 on error.
The RSA\_padding\_check\_xxx() functions return the length of the
recovered data, -1 on error. Error codes can be obtained by calling
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# SEE ALSO

[RSA\_public\_encrypt(3)](http://man.he.net/man3/RSA_public_encrypt),
[RSA\_private\_decrypt(3)](http://man.he.net/man3/RSA_private_decrypt),
[RSA\_sign(3)](http://man.he.net/man3/RSA_sign), [RSA\_verify(3)](http://man.he.net/man3/RSA_verify)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
