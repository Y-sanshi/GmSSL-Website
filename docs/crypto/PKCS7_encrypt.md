# NAME

PKCS7\_encrypt - create a PKCS#7 envelopedData structure

# SYNOPSIS

    #include <openssl/pkcs7.h>

    PKCS7 *PKCS7_encrypt(STACK_OF(X509) *certs, BIO *in, const EVP_CIPHER *cipher, int flags);

# DESCRIPTION

PKCS7\_encrypt() creates and returns a PKCS#7 envelopedData structure. **certs**
is a list of recipient certificates. **in** is the content to be encrypted.
**cipher** is the symmetric cipher to use. **flags** is an optional set of flags.

# NOTES

Only RSA keys are supported in PKCS#7 and envelopedData so the recipient
certificates supplied to this function must all contain RSA public keys, though
they do not have to be signed using the RSA algorithm.

EVP\_des\_ede3\_cbc() (triple DES) is the algorithm of choice for S/MIME use
because most clients will support it.

Some old "export grade" clients may only support weak encryption using 40 or 64
bit RC2. These can be used by passing EVP\_rc2\_40\_cbc() and EVP\_rc2\_64\_cbc()
respectively.

The algorithm passed in the **cipher** parameter must support ASN1 encoding of
its parameters.

Many browsers implement a "sign and encrypt" option which is simply an S/MIME
envelopedData containing an S/MIME signed message. This can be readily produced
by storing the S/MIME signed message in a memory BIO and passing it to
PKCS7\_encrypt().

The following flags can be passed in the **flags** parameter.

If the **PKCS7\_TEXT** flag is set MIME headers for type **text/plain** are
prepended to the data.

Normally the supplied content is translated into MIME canonical format (as
required by the S/MIME specifications) if **PKCS7\_BINARY** is set no translation
occurs. This option should be used if the supplied data is in binary format
otherwise the translation will corrupt it. If **PKCS7\_BINARY** is set then
**PKCS7\_TEXT** is ignored.

If the **PKCS7\_STREAM** flag is set a partial **PKCS7** structure is output
suitable for streaming I/O: no data is read from the BIO **in**.

# NOTES

If the flag **PKCS7\_STREAM** is set the returned **PKCS7** structure is **not**
complete and outputting its contents via a function that does not
properly finalize the **PKCS7** structure will give unpredictable
results.

Several functions including SMIME\_write\_PKCS7(), i2d\_PKCS7\_bio\_stream(),
PEM\_write\_bio\_PKCS7\_stream() finalize the structure. Alternatively finalization
can be performed by obtaining the streaming ASN1 **BIO** directly using
BIO\_new\_PKCS7().

# RETURN VALUES

PKCS7\_encrypt() returns either a PKCS7 structure or NULL if an error occurred.
The error can be obtained from ERR\_get\_error(3).

# SEE ALSO

[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error), [PKCS7\_decrypt(3)](http://man.he.net/man3/PKCS7_decrypt)

# HISTORY

The **PKCS7\_STREAM** flag was added in OpenSSL 1.0.0.

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
