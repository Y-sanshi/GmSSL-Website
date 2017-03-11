# NAME

PEM\_write, PEM\_write\_bio,
PEM\_read, PEM\_read\_bio, PEM\_do\_header, PEM\_get\_EVP\_CIPHER\_INFO
\- PEM encoding routines

# SYNOPSIS

    #include <openssl/pem.h>

    int PEM_write(FILE *fp, const char *name, const char *header,
                  const unsigned char *data, long len)
    int PEM_write_bio(BIO *bp, const char *name, const char *header,
                      const unsigned char *data, long len)

    int PEM_read(FILE *fp, char **name, char **header,
                 unsigned char **data, long *len);
    int PEM_read_bio(BIO *bp, char **name, char **header,
                     unsigned char **data, long *len);

    int PEM_get_EVP_CIPHER_INFO(char *header, EVP_CIPHER_INFO *cinfo);
    int PEM_do_header(EVP_CIPHER_INFO *cinfo, unsigned char *data, long *len,
                      pem_password_cb *cb, void *u);

# DESCRIPTION

These functions read and write PEM-encoded objects, using the PEM
type **name**, any additional **header** information, and the raw
**data** of length **len**.

PEM is the term used for binary content encoding first defined in IETF
RFC 1421.  The content is a series of base64-encoded lines, surrounded
by begin/end markers each on their own line.  For example:

    -----BEGIN PRIVATE KEY-----
    MIICdg....
    ... bhTQ==
    -----END PRIVATE KEY-----

Optional header line(s) may appear after the begin line, and their
existence depends on the type of object being written or read.

PEM\_write() writes to the file **fp**, while PEM\_write\_bio() writes to
the BIO **bp**.  The **name** is the name to use in the marker, the
**header** is the header value or NULL, and **data** and **len** specify
the data and its length.

The final **data** buffer is typically an ASN.1 object which can be decoded with
the **d2i** function appropriate to the type **name**; see [d2i\_X509(3)](http://man.he.net/man3/d2i_X509)
for examples.

PEM\_read() reads from the file **fp**, while PEM\_read\_bio() reads
from the BIO **bp**.
Both skip any non-PEM data that precedes the start of the next PEM object.
When an object is successfully retrieved, the type name from the "----BEGIN
&lt;type>-----" is returned via the **name** argument, any encapsulation headers
are returned in **header** and the base64-decoded content and its length are
returned via **data** and **len** respectively.
The **name**, **header** and **data** pointers are allocated via OPENSSL\_malloc()
and should be freed by the caller via OPENSSL\_free() when no longer needed.

PEM\_get\_EVP\_CIPHER\_INFO() can be used to determine the **data** returned by
PEM\_read() or PEM\_read\_bio() is encrypted and to retrieve the associated cipher
and IV.
The caller passes a pointer to structure of type **EVP\_CIPHER\_INFO** via the
**cinfo** argument and the **header** returned via PEM\_read() or PEM\_read\_bio().
If the call is successful 1 is returned and the cipher and IV are stored at the
address pointed to by **cinfo**.
When the header is malformed, or not supported or when the cipher is unknown
or some internal error happens 0 is returned.
This function is deprecated, see **NOTES** below.

PEM\_do\_header() can then be used to decrypt the data if the header
indicates encryption.
The **cinfo** argument is a pointer to the structure initialized by the previous
call to PEM\_get\_EVP\_CIPHER\_INFO().
The **data** and **len** arguments are those returned by the previous call to
PEM\_read() or PEM\_read\_bio().
The **cb** and **u** arguments make it possible to override the default password
prompt function as described in [PEM\_read\_PrivateKey(3)](http://man.he.net/man3/PEM_read_PrivateKey).
On successful completion the **data** is decrypted in place, and **len** is
updated to indicate the plaintext length.
This function is deprecated, see **NOTES** below.

If the data is a priori known to not be encrypted, then neither PEM\_do\_header()
nor PEM\_get\_EVP\_CIPHER\_INFO() need be called.

# RETURN VALUES

PEM\_read() and PEM\_read\_bio() return 1 on success and 0 on failure, the latter
includes the case when no more PEM objects remain in the input file.
To distinguish end of file from more serious errors the caller must peek at the
error stack and check for **PEM\_R\_NO\_START\_LINE**, which indicates that no more
PEM objects were found.  See [ERR\_peek\_last\_error(3)](http://man.he.net/man3/ERR_peek_last_error), [ERR\_GET\_REASON(3)](http://man.he.net/man3/ERR_GET_REASON).

PEM\_get\_EVP\_CIPHER\_INFO() and PEM\_do\_header() return 1 on success, and 0 on
failure.
The **data** is likely meaningless if these functions fail.

# NOTES

The PEM\_get\_EVP\_CIPHER\_INFO() and PEM\_do\_header() functions are deprecated.
This is because the underlying PEM encryption format is obsolete, and should
be avoided.
It uses an encryption format with an OpenSSL-specific key-derivation function,
which employs MD5 with an iteration count of 1!
Instead, private keys should be stored in PKCS#8 form, with a strong PKCS#5
v2.0 PBE.
See [PEM\_write\_PrivateKey(3)](http://man.he.net/man3/PEM_write_PrivateKey) and [d2i\_PKCS8PrivateKey\_bio(3)](http://man.he.net/man3/d2i_PKCS8PrivateKey_bio).

# SEE ALSO

[ERR\_peek\_last\_error(3)](http://man.he.net/man3/ERR_peek_last_error), [ERR\_GET\_LIB(3)](http://man.he.net/man3/ERR_GET_LIB),
[d2i\_PKCS8PrivateKey\_bio(3)](http://man.he.net/man3/d2i_PKCS8PrivateKey_bio).

# COPYRIGHT

Copyright 1998-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
