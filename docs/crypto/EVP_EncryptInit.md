# NAME

EVP\_CIPHER\_CTX\_new, EVP\_CIPHER\_CTX\_reset, EVP\_CIPHER\_CTX\_free,
EVP\_EncryptInit\_ex, EVP\_EncryptUpdate, EVP\_EncryptFinal\_ex,
EVP\_DecryptInit\_ex, EVP\_DecryptUpdate, EVP\_DecryptFinal\_ex,
EVP\_CipherInit\_ex, EVP\_CipherUpdate, EVP\_CipherFinal\_ex,
EVP\_CIPHER\_CTX\_set\_key\_length, EVP\_CIPHER\_CTX\_ctrl, EVP\_EncryptInit,
EVP\_EncryptFinal, EVP\_DecryptInit, EVP\_DecryptFinal,
EVP\_CipherInit, EVP\_CipherFinal, EVP\_get\_cipherbyname,
EVP\_get\_cipherbynid, EVP\_get\_cipherbyobj, EVP\_CIPHER\_nid,
EVP\_CIPHER\_block\_size, EVP\_CIPHER\_key\_length, EVP\_CIPHER\_iv\_length,
EVP\_CIPHER\_flags, EVP\_CIPHER\_mode, EVP\_CIPHER\_type, EVP\_CIPHER\_CTX\_cipher,
EVP\_CIPHER\_CTX\_nid, EVP\_CIPHER\_CTX\_block\_size, EVP\_CIPHER\_CTX\_key\_length,
EVP\_CIPHER\_CTX\_iv\_length, EVP\_CIPHER\_CTX\_get\_app\_data,
EVP\_CIPHER\_CTX\_set\_app\_data, EVP\_CIPHER\_CTX\_type, EVP\_CIPHER\_CTX\_flags,
EVP\_CIPHER\_CTX\_mode, EVP\_CIPHER\_param\_to\_asn1, EVP\_CIPHER\_asn1\_to\_param,
EVP\_CIPHER\_CTX\_set\_padding, EVP\_enc\_null, EVP\_des\_cbc, EVP\_des\_ecb,
EVP\_des\_cfb, EVP\_des\_ofb, EVP\_des\_ede\_cbc, EVP\_des\_ede, EVP\_des\_ede\_ofb,
EVP\_des\_ede\_cfb, EVP\_des\_ede3\_cbc, EVP\_des\_ede3, EVP\_des\_ede3\_ofb,
EVP\_des\_ede3\_cfb, EVP\_desx\_cbc, EVP\_rc4, EVP\_rc4\_40, EVP\_idea\_cbc,
EVP\_idea\_ecb, EVP\_idea\_cfb, EVP\_idea\_ofb, EVP\_rc2\_cbc,
EVP\_rc2\_ecb, EVP\_rc2\_cfb, EVP\_rc2\_ofb, EVP\_rc2\_40\_cbc, EVP\_rc2\_64\_cbc,
EVP\_bf\_cbc, EVP\_bf\_ecb, EVP\_bf\_cfb, EVP\_bf\_ofb, EVP\_cast5\_cbc,
EVP\_cast5\_ecb, EVP\_cast5\_cfb, EVP\_cast5\_ofb, EVP\_rc5\_32\_12\_16\_cbc,
EVP\_rc5\_32\_12\_16\_ecb, EVP\_rc5\_32\_12\_16\_cfb, EVP\_rc5\_32\_12\_16\_ofb,
EVP\_aes\_128\_cbc, EVP\_aes\_128\_ecb, EVP\_aes\_128\_cfb, EVP\_aes\_128\_ofb,
EVP\_aes\_192\_cbc, EVP\_aes\_192\_ecb, EVP\_aes\_192\_cfb, EVP\_aes\_192\_ofb,
EVP\_aes\_256\_cbc, EVP\_aes\_256\_ecb, EVP\_aes\_256\_cfb, EVP\_aes\_256\_ofb,
EVP\_aes\_128\_gcm, EVP\_aes\_192\_gcm, EVP\_aes\_256\_gcm,
EVP\_aes\_128\_ccm, EVP\_aes\_192\_ccm, EVP\_aes\_256\_ccm,
EVP\_chacha20, EVP\_chacha20\_poly1305 - EVP cipher routines

# SYNOPSIS

    #include <openssl/evp.h>

    EVP_CIPHER_CTX *EVP_CIPHER_CTX_new(void);
    int EVP_CIPHER_CTX_reset(EVP_CIPHER_CTX *ctx);
    void EVP_CIPHER_CTX_free(EVP_CIPHER_CTX *ctx);

    int EVP_EncryptInit_ex(EVP_CIPHER_CTX *ctx, const EVP_CIPHER *type,
            ENGINE *impl, unsigned char *key, unsigned char *iv);
    int EVP_EncryptUpdate(EVP_CIPHER_CTX *ctx, unsigned char *out,
            int *outl, unsigned char *in, int inl);
    int EVP_EncryptFinal_ex(EVP_CIPHER_CTX *ctx, unsigned char *out,
            int *outl);

    int EVP_DecryptInit_ex(EVP_CIPHER_CTX *ctx, const EVP_CIPHER *type,
            ENGINE *impl, unsigned char *key, unsigned char *iv);
    int EVP_DecryptUpdate(EVP_CIPHER_CTX *ctx, unsigned char *out,
            int *outl, unsigned char *in, int inl);
    int EVP_DecryptFinal_ex(EVP_CIPHER_CTX *ctx, unsigned char *outm,
            int *outl);

    int EVP_CipherInit_ex(EVP_CIPHER_CTX *ctx, const EVP_CIPHER *type,
            ENGINE *impl, unsigned char *key, unsigned char *iv, int enc);
    int EVP_CipherUpdate(EVP_CIPHER_CTX *ctx, unsigned char *out,
            int *outl, unsigned char *in, int inl);
    int EVP_CipherFinal_ex(EVP_CIPHER_CTX *ctx, unsigned char *outm,
            int *outl);

    int EVP_EncryptInit(EVP_CIPHER_CTX *ctx, const EVP_CIPHER *type,
            unsigned char *key, unsigned char *iv);
    int EVP_EncryptFinal(EVP_CIPHER_CTX *ctx, unsigned char *out,
            int *outl);

    int EVP_DecryptInit(EVP_CIPHER_CTX *ctx, const EVP_CIPHER *type,
            unsigned char *key, unsigned char *iv);
    int EVP_DecryptFinal(EVP_CIPHER_CTX *ctx, unsigned char *outm,
            int *outl);

    int EVP_CipherInit(EVP_CIPHER_CTX *ctx, const EVP_CIPHER *type,
            unsigned char *key, unsigned char *iv, int enc);
    int EVP_CipherFinal(EVP_CIPHER_CTX *ctx, unsigned char *outm,
            int *outl);

    int EVP_CIPHER_CTX_set_padding(EVP_CIPHER_CTX *x, int padding);
    int EVP_CIPHER_CTX_set_key_length(EVP_CIPHER_CTX *x, int keylen);
    int EVP_CIPHER_CTX_ctrl(EVP_CIPHER_CTX *ctx, int type, int arg, void *ptr);

    const EVP_CIPHER *EVP_get_cipherbyname(const char *name);
    const EVP_CIPHER *EVP_get_cipherbynid(int nid);
    const EVP_CIPHER *EVP_get_cipherbyobj(const ASN1_OBJECT *a);

    int EVP_CIPHER_nid(const EVP_CIPHER *e);
    int EVP_CIPHER_block_size(const EVP_CIPHER *e);
    int EVP_CIPHER_key_length(const EVP_CIPHER *e)
    int EVP_CIPHER_key_length(const EVP_CIPHER *e);
    int EVP_CIPHER_iv_length(const EVP_CIPHER *e);
    unsigned long EVP_CIPHER_flags(const EVP_CIPHER *e);
    unsigned long EVP_CIPHER_mode(const EVP_CIPHER *e);
    int EVP_CIPHER_type(const EVP_CIPHER *ctx);

    const EVP_CIPHER *EVP_CIPHER_CTX_cipher(const EVP_CIPHER_CTX *ctx);
    int EVP_CIPHER_CTX_nid(const EVP_CIPHER_CTX *ctx);
    int EVP_CIPHER_CTX_block_size(const EVP_CIPHER_CTX *ctx);
    int EVP_CIPHER_CTX_key_length(const EVP_CIPHER_CTX *ctx);
    int EVP_CIPHER_CTX_iv_length(const EVP_CIPHER_CTX *ctx);
    void *EVP_CIPHER_CTX_get_app_data(const EVP_CIPHER_CTX *ctx);
    void EVP_CIPHER_CTX_set_app_data(const EVP_CIPHER_CTX *ctx, void *data);
    int EVP_CIPHER_CTX_type(const EVP_CIPHER_CTX *ctx);
    int EVP_CIPHER_CTX_mode(const EVP_CIPHER_CTX *ctx);

    int EVP_CIPHER_param_to_asn1(EVP_CIPHER_CTX *c, ASN1_TYPE *type);
    int EVP_CIPHER_asn1_to_param(EVP_CIPHER_CTX *c, ASN1_TYPE *type);

# DESCRIPTION

The EVP cipher routines are a high level interface to certain
symmetric ciphers.

EVP\_CIPHER\_CTX\_new() creates a cipher context.

EVP\_CIPHER\_CTX\_free() clears all information from a cipher context
and free up any allocated memory associate with it, including **ctx**
itself. This function should be called after all operations using a
cipher are complete so sensitive information does not remain in
memory.

EVP\_EncryptInit\_ex() sets up cipher context **ctx** for encryption
with cipher **type** from ENGINE **impl**. **ctx** must be created
before calling this function. **type** is normally supplied
by a function such as EVP\_aes\_256\_cbc(). If **impl** is NULL then the
default implementation is used. **key** is the symmetric key to use
and **iv** is the IV to use (if necessary), the actual number of bytes
used for the key and IV depends on the cipher. It is possible to set
all parameters to NULL except **type** in an initial call and supply
the remaining parameters in subsequent calls, all of which have **type**
set to NULL. This is done when the default cipher parameters are not
appropriate.

EVP\_EncryptUpdate() encrypts **inl** bytes from the buffer **in** and
writes the encrypted version to **out**. This function can be called
multiple times to encrypt successive blocks of data. The amount
of data written depends on the block alignment of the encrypted data:
as a result the amount of data written may be anything from zero bytes
to (inl + cipher\_block\_size - 1) so **out** should contain sufficient
room. The actual number of bytes written is placed in **outl**. It also
checks if **in** and **out** are partially overlapping, and if they are
0 is returned to indicate failure.

If padding is enabled (the default) then EVP\_EncryptFinal\_ex() encrypts
the "final" data, that is any data that remains in a partial block.
It uses standard block padding (aka PKCS padding) as described in
the NOTES section, below. The encrypted
final data is written to **out** which should have sufficient space for
one cipher block. The number of bytes written is placed in **outl**. After
this function is called the encryption operation is finished and no further
calls to EVP\_EncryptUpdate() should be made.

If padding is disabled then EVP\_EncryptFinal\_ex() will not encrypt any more
data and it will return an error if any data remains in a partial block:
that is if the total data length is not a multiple of the block size.

EVP\_DecryptInit\_ex(), EVP\_DecryptUpdate() and EVP\_DecryptFinal\_ex() are the
corresponding decryption operations. EVP\_DecryptFinal() will return an
error code if padding is enabled and the final block is not correctly
formatted. The parameters and restrictions are identical to the encryption
operations except that if padding is enabled the decrypted data buffer **out**
passed to EVP\_DecryptUpdate() should have sufficient room for
(**inl** + cipher\_block\_size) bytes unless the cipher block size is 1 in
which case **inl** bytes is sufficient.

EVP\_CipherInit\_ex(), EVP\_CipherUpdate() and EVP\_CipherFinal\_ex() are
functions that can be used for decryption or encryption. The operation
performed depends on the value of the **enc** parameter. It should be set
to 1 for encryption, 0 for decryption and -1 to leave the value unchanged
(the actual value of 'enc' being supplied in a previous call).

EVP\_CIPHER\_CTX\_reset() clears all information from a cipher context
and free up any allocated memory associate with it, except the **ctx**
itself. This function should be called anytime **ctx** is to be reused
for another EVP\_CipherInit() / EVP\_CipherUpdate() / EVP\_CipherFinal()
series of calls.

EVP\_EncryptInit(), EVP\_DecryptInit() and EVP\_CipherInit() behave in a
similar way to EVP\_EncryptInit\_ex(), EVP\_DecryptInit\_ex() and
EVP\_CipherInit\_ex() except the **ctx** parameter does not need to be
initialized and they always use the default cipher implementation.

EVP\_EncryptFinal(), EVP\_DecryptFinal() and EVP\_CipherFinal() are
identical to EVP\_EncryptFinal\_ex(), EVP\_DecryptFinal\_ex() and
EVP\_CipherFinal\_ex(). In previous releases they also cleaned up
the **ctx**, but this is no longer done and EVP\_CIPHER\_CTX\_clean()
must be called to free any context resources.

EVP\_get\_cipherbyname(), EVP\_get\_cipherbynid() and EVP\_get\_cipherbyobj()
return an EVP\_CIPHER structure when passed a cipher name, a NID or an
ASN1\_OBJECT structure.

EVP\_CIPHER\_nid() and EVP\_CIPHER\_CTX\_nid() return the NID of a cipher when
passed an **EVP\_CIPHER** or **EVP\_CIPHER\_CTX** structure.  The actual NID
value is an internal value which may not have a corresponding OBJECT
IDENTIFIER.

EVP\_CIPHER\_CTX\_set\_padding() enables or disables padding. This
function should be called after the context is set up for encryption
or decryption with EVP\_EncryptInit\_ex(), EVP\_DecryptInit\_ex() or
EVP\_CipherInit\_ex(). By default encryption operations are padded using
standard block padding and the padding is checked and removed when
decrypting. If the **pad** parameter is zero then no padding is
performed, the total amount of data encrypted or decrypted must then
be a multiple of the block size or an error will occur.

EVP\_CIPHER\_key\_length() and EVP\_CIPHER\_CTX\_key\_length() return the key
length of a cipher when passed an **EVP\_CIPHER** or **EVP\_CIPHER\_CTX**
structure. The constant **EVP\_MAX\_KEY\_LENGTH** is the maximum key length
for all ciphers. Note: although EVP\_CIPHER\_key\_length() is fixed for a
given cipher, the value of EVP\_CIPHER\_CTX\_key\_length() may be different
for variable key length ciphers.

EVP\_CIPHER\_CTX\_set\_key\_length() sets the key length of the cipher ctx.
If the cipher is a fixed length cipher then attempting to set the key
length to any value other than the fixed value is an error.

EVP\_CIPHER\_iv\_length() and EVP\_CIPHER\_CTX\_iv\_length() return the IV
length of a cipher when passed an **EVP\_CIPHER** or **EVP\_CIPHER\_CTX**.
It will return zero if the cipher does not use an IV.  The constant
**EVP\_MAX\_IV\_LENGTH** is the maximum IV length for all ciphers.

EVP\_CIPHER\_block\_size() and EVP\_CIPHER\_CTX\_block\_size() return the block
size of a cipher when passed an **EVP\_CIPHER** or **EVP\_CIPHER\_CTX**
structure. The constant **EVP\_MAX\_BLOCK\_LENGTH** is also the maximum block
length for all ciphers.

EVP\_CIPHER\_type() and EVP\_CIPHER\_CTX\_type() return the type of the passed
cipher or context. This "type" is the actual NID of the cipher OBJECT
IDENTIFIER as such it ignores the cipher parameters and 40 bit RC2 and
128 bit RC2 have the same NID. If the cipher does not have an object
identifier or does not have ASN1 support this function will return
**NID\_undef**.

EVP\_CIPHER\_CTX\_cipher() returns the **EVP\_CIPHER** structure when passed
an **EVP\_CIPHER\_CTX** structure.

EVP\_CIPHER\_mode() and EVP\_CIPHER\_CTX\_mode() return the block cipher mode:
EVP\_CIPH\_ECB\_MODE, EVP\_CIPH\_CBC\_MODE, EVP\_CIPH\_CFB\_MODE or
EVP\_CIPH\_OFB\_MODE. If the cipher is a stream cipher then
EVP\_CIPH\_STREAM\_CIPHER is returned.

EVP\_CIPHER\_param\_to\_asn1() sets the AlgorithmIdentifier "parameter" based
on the passed cipher. This will typically include any parameters and an
IV. The cipher IV (if any) must be set when this call is made. This call
should be made before the cipher is actually "used" (before any
EVP\_EncryptUpdate(), EVP\_DecryptUpdate() calls for example). This function
may fail if the cipher does not have any ASN1 support.

EVP\_CIPHER\_asn1\_to\_param() sets the cipher parameters based on an ASN1
AlgorithmIdentifier "parameter". The precise effect depends on the cipher
In the case of RC2, for example, it will set the IV and effective key length.
This function should be called after the base cipher type is set but before
the key is set. For example EVP\_CipherInit() will be called with the IV and
key set to NULL, EVP\_CIPHER\_asn1\_to\_param() will be called and finally
EVP\_CipherInit() again with all parameters except the key set to NULL. It is
possible for this function to fail if the cipher does not have any ASN1 support
or the parameters cannot be set (for example the RC2 effective key length
is not supported.

EVP\_CIPHER\_CTX\_ctrl() allows various cipher specific parameters to be determined
and set.

# RETURN VALUES

EVP\_CIPHER\_CTX\_new() returns a pointer to a newly created
**EVP\_CIPHER\_CTX** for success and **NULL** for failure.

EVP\_EncryptInit\_ex(), EVP\_EncryptUpdate() and EVP\_EncryptFinal\_ex()
return 1 for success and 0 for failure.

EVP\_DecryptInit\_ex() and EVP\_DecryptUpdate() return 1 for success and 0 for failure.
EVP\_DecryptFinal\_ex() returns 0 if the decrypt failed or 1 for success.

EVP\_CipherInit\_ex() and EVP\_CipherUpdate() return 1 for success and 0 for failure.
EVP\_CipherFinal\_ex() returns 0 for a decryption failure or 1 for success.

EVP\_CIPHER\_CTX\_reset() returns 1 for success and 0 for failure.

EVP\_get\_cipherbyname(), EVP\_get\_cipherbynid() and EVP\_get\_cipherbyobj()
return an **EVP\_CIPHER** structure or NULL on error.

EVP\_CIPHER\_nid() and EVP\_CIPHER\_CTX\_nid() return a NID.

EVP\_CIPHER\_block\_size() and EVP\_CIPHER\_CTX\_block\_size() return the block
size.

EVP\_CIPHER\_key\_length() and EVP\_CIPHER\_CTX\_key\_length() return the key
length.

EVP\_CIPHER\_CTX\_set\_padding() always returns 1.

EVP\_CIPHER\_iv\_length() and EVP\_CIPHER\_CTX\_iv\_length() return the IV
length or zero if the cipher does not use an IV.

EVP\_CIPHER\_type() and EVP\_CIPHER\_CTX\_type() return the NID of the cipher's
OBJECT IDENTIFIER or NID\_undef if it has no defined OBJECT IDENTIFIER.

EVP\_CIPHER\_CTX\_cipher() returns an **EVP\_CIPHER** structure.

EVP\_CIPHER\_param\_to\_asn1() and EVP\_CIPHER\_asn1\_to\_param() return greater
than zero for success and zero or a negative number.

# CIPHER LISTING

All algorithms have a fixed key length unless otherwise stated.

- EVP\_enc\_null()

    Null cipher: does nothing.

- EVP\_aes\_128\_cbc(), EVP\_aes\_128\_ecb(), EVP\_aes\_128\_cfb(), EVP\_aes\_128\_ofb()

    AES with a 128-bit key in CBC, ECB, CFB and OFB modes respectively.

- EVP\_aes\_192\_cbc(), EVP\_aes\_192\_ecb(), EVP\_aes\_192\_cfb(), EVP\_aes\_192\_ofb()

    AES with a 192-bit key in CBC, ECB, CFB and OFB modes respectively.

- EVP\_aes\_256\_cbc(), EVP\_aes\_256\_ecb(), EVP\_aes\_256\_cfb(), EVP\_aes\_256\_ofb()

    AES with a 256-bit key in CBC, ECB, CFB and OFB modes respectively.

- EVP\_des\_cbc(), EVP\_des\_ecb(), EVP\_des\_cfb(), EVP\_des\_ofb()

    DES in CBC, ECB, CFB and OFB modes respectively.

- EVP\_des\_ede\_cbc(), EVP\_des\_ede(), EVP\_des\_ede\_ofb(), EVP\_des\_ede\_cfb()

    Two key triple DES in CBC, ECB, CFB and OFB modes respectively.

- EVP\_des\_ede3\_cbc(), EVP\_des\_ede3(), EVP\_des\_ede3\_ofb(), EVP\_des\_ede3\_cfb()

    Three key triple DES in CBC, ECB, CFB and OFB modes respectively.

- EVP\_desx\_cbc()

    DESX algorithm in CBC mode.

- EVP\_rc4()

    RC4 stream cipher. This is a variable key length cipher with default key length 128 bits.

- EVP\_rc4\_40()

    RC4 stream cipher with 40 bit key length.
    This is obsolete and new code should use EVP\_rc4()
    and the EVP\_CIPHER\_CTX\_set\_key\_length() function.

- EVP\_idea\_cbc() EVP\_idea\_ecb(), EVP\_idea\_cfb(), EVP\_idea\_ofb()

    IDEA encryption algorithm in CBC, ECB, CFB and OFB modes respectively.

- EVP\_rc2\_cbc(), EVP\_rc2\_ecb(), EVP\_rc2\_cfb(), EVP\_rc2\_ofb()

    RC2 encryption algorithm in CBC, ECB, CFB and OFB modes respectively. This is a variable key
    length cipher with an additional parameter called "effective key bits" or "effective key length".
    By default both are set to 128 bits.

- EVP\_rc2\_40\_cbc(), EVP\_rc2\_64\_cbc()

    RC2 algorithm in CBC mode with a default key length and effective key length of 40 and 64 bits.
    These are obsolete and new code should use EVP\_rc2\_cbc(), EVP\_CIPHER\_CTX\_set\_key\_length() and
    EVP\_CIPHER\_CTX\_ctrl() to set the key length and effective key length.

- EVP\_bf\_cbc(), EVP\_bf\_ecb(), EVP\_bf\_cfb(), EVP\_bf\_ofb()

    Blowfish encryption algorithm in CBC, ECB, CFB and OFB modes respectively. This is a variable key
    length cipher.

- EVP\_cast5\_cbc(), EVP\_cast5\_ecb(), EVP\_cast5\_cfb(), EVP\_cast5\_ofb()

    CAST encryption algorithm in CBC, ECB, CFB and OFB modes respectively. This is a variable key
    length cipher.

- EVP\_rc5\_32\_12\_16\_cbc(), EVP\_rc5\_32\_12\_16\_ecb(), EVP\_rc5\_32\_12\_16\_cfb(), EVP\_rc5\_32\_12\_16\_ofb()

    RC5 encryption algorithm in CBC, ECB, CFB and OFB modes respectively. This is a variable key length
    cipher with an additional "number of rounds" parameter. By default the key length is set to 128
    bits and 12 rounds.

- EVP\_aes\_128\_gcm(), EVP\_aes\_192\_gcm(), EVP\_aes\_256\_gcm()

    AES Galois Counter Mode (GCM) for 128, 192 and 256 bit keys respectively.
    These ciphers require additional control operations to function correctly: see
    the ["GCM and OCB Modes"](#gcm-and-ocb-modes) section below for details.

- EVP\_aes\_128\_ocb(void), EVP\_aes\_192\_ocb(void), EVP\_aes\_256\_ocb(void)

    Offset Codebook Mode (OCB) for 128, 192 and 256 bit keys respectively.
    These ciphers require additional control operations to function correctly: see
    the ["GCM and OCB Modes"](#gcm-and-ocb-modes) section below for details.

- EVP\_aes\_128\_ccm(), EVP\_aes\_192\_ccm(), EVP\_aes\_256\_ccm()

    AES Counter with CBC-MAC Mode (CCM) for 128, 192 and 256 bit keys respectively.
    These ciphers require additional control operations to function correctly: see
    CCM mode section below for details.

- EVP\_chacha20()

    The ChaCha20 stream cipher. The key length is 256 bits, the IV is 96 bits long.

- EVP\_chacha20\_poly1305()

    Authenticated encryption with ChaCha20-Poly1305. Like EVP\_chacha20() the key is
    256 bits and the IV is 96 bits. This supports additional authenticated
    data (AAD) and produces a 128 bit authentication tag. See the
    ["GCM and OCB Modes"](#gcm-and-ocb-modes) section for more information.

# GCM and OCB Modes

For GCM and OCB mode ciphers the behaviour of the EVP interface is subtly
altered and several additional ctrl operations are supported.

To specify any additional authenticated data (AAD) a call to EVP\_CipherUpdate(),
EVP\_EncryptUpdate() or EVP\_DecryptUpdate() should be made with the output
parameter **out** set to **NULL**.

When decrypting the return value of EVP\_DecryptFinal() or EVP\_CipherFinal()
indicates if the operation was successful. If it does not indicate success
the authentication operation has failed and any output data **MUST NOT**
be used as it is corrupted.

The following ctrls are supported in both GCM and OCB modes:

    EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_AEAD_SET_IVLEN, ivlen, NULL);

Sets the IV length: this call can only be made before specifying an IV. If
not called a default IV length is used. For GCM AES and OCB AES the default is
12 (i.e. 96 bits). For OCB mode the maximum is 15.

    EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_AEAD_GET_TAG, taglen, tag);

Writes **taglen** bytes of the tag value to the buffer indicated by **tag**.
This call can only be made when encrypting data and **after** all data has been
processed (e.g. after an EVP\_EncryptFinal() call). For OCB mode the taglen must
either be 16 or the value previously set via EVP\_CTRL\_OCB\_SET\_TAGLEN.

    EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_AEAD_SET_TAG, taglen, tag);

Sets the expected tag to **taglen** bytes from **tag**. This call is only legal
when decrypting data and must be made **before** any data is processed (e.g.
before any EVP\_DecryptUpdate() call). For OCB mode the taglen must
either be 16 or the value previously set via EVP\_CTRL\_AEAD\_SET\_TAG.

In OCB mode calling this with **tag** set to NULL sets the tag length. The tag
length can only be set before specifying an IV. If not called a default tag
length is used. For OCB AES the default is 16 (i.e. 128 bits). This is also the
maximum tag length for OCB.

See ["EXAMPLES"](#examples) below for an example of the use of GCM mode.

# CCM Mode

The behaviour of CCM mode ciphers is similar to GCM mode but with a few
additional requirements and different ctrl values.

Like GCM and OCB modes any additional authenticated data (AAD) is passed by calling
EVP\_CipherUpdate(), EVP\_EncryptUpdate() or EVP\_DecryptUpdate() with the output
parameter **out** set to **NULL**. Additionally the total plaintext or ciphertext
length **MUST** be passed to EVP\_CipherUpdate(), EVP\_EncryptUpdate() or
EVP\_DecryptUpdate() with the output and input parameters (**in** and **out**)
set to **NULL** and the length passed in the **inl** parameter.

The following ctrls are supported in CCM mode:

    EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_AEAD_SET_TAG, taglen, tag);

This call is made to set the expected **CCM** tag value when decrypting or
the length of the tag (with the **tag** parameter set to NULL) when encrypting.
The tag length is often referred to as **M**. If not set a default value is
used (12 for AES).

    EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_CCM_SET_L, ivlen, NULL);

Sets the CCM **L** value. If not set a default is used (8 for AES).

    EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_AEAD_SET_IVLEN, ivlen, NULL);

Sets the CCM nonce (IV) length: this call can only be made before specifying
an nonce value. The nonce length is given by **15 - L** so it is 7 by default
for AES.

# NOTES

Where possible the **EVP** interface to symmetric ciphers should be used in
preference to the low level interfaces. This is because the code then becomes
transparent to the cipher used and much more flexible. Additionally, the
**EVP** interface will ensure the use of platform specific cryptographic
acceleration such as AES-NI (the low level interfaces do not provide the
guarantee).

PKCS padding works by adding **n** padding bytes of value **n** to make the total
length of the encrypted data a multiple of the block size. Padding is always
added so if the data is already a multiple of the block size **n** will equal
the block size. For example if the block size is 8 and 11 bytes are to be
encrypted then 5 padding bytes of value 5 will be added.

When decrypting the final block is checked to see if it has the correct form.

Although the decryption operation can produce an error if padding is enabled,
it is not a strong test that the input data or key is correct. A random block
has better than 1 in 256 chance of being of the correct format and problems with
the input data earlier on will not produce a final decrypt error.

If padding is disabled then the decryption operation will always succeed if
the total amount of data decrypted is a multiple of the block size.

The functions EVP\_EncryptInit(), EVP\_EncryptFinal(), EVP\_DecryptInit(),
EVP\_CipherInit() and EVP\_CipherFinal() are obsolete but are retained for
compatibility with existing code. New code should use EVP\_EncryptInit\_ex(),
EVP\_EncryptFinal\_ex(), EVP\_DecryptInit\_ex(), EVP\_DecryptFinal\_ex(),
EVP\_CipherInit\_ex() and EVP\_CipherFinal\_ex() because they can reuse an
existing context without allocating and freeing it up on each call.

EVP\_get\_cipherbynid(), and EVP\_get\_cipherbyobj() are implemented as macros.

# BUGS

For RC5 the number of rounds can currently only be set to 8, 12 or 16. This is
a limitation of the current RC5 code rather than the EVP interface.

EVP\_MAX\_KEY\_LENGTH and EVP\_MAX\_IV\_LENGTH only refer to the internal ciphers with
default key lengths. If custom ciphers exceed these values the results are
unpredictable. This is because it has become standard practice to define a
generic key as a fixed unsigned char array containing EVP\_MAX\_KEY\_LENGTH bytes.

The ASN1 code is incomplete (and sometimes inaccurate) it has only been tested
for certain common S/MIME ciphers (RC2, DES, triple DES) in CBC mode.

# EXAMPLES

Encrypt a string using IDEA:

    int do_crypt(char *outfile)
           {
           unsigned char outbuf[1024];
           int outlen, tmplen;
           /* Bogus key and IV: we'd normally set these from
            * another source.
            */
           unsigned char key[] = {0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15};
           unsigned char iv[] = {1,2,3,4,5,6,7,8};
           char intext[] = "Some Crypto Text";
           EVP_CIPHER_CTX ctx;
           FILE *out;

           ctx = EVP_CIPHER_CTX_new();
           EVP_EncryptInit_ex(ctx, EVP_idea_cbc(), NULL, key, iv);

           if(!EVP_EncryptUpdate(ctx, outbuf, &outlen, intext, strlen(intext)))
                   {
                   /* Error */
                   return 0;
                   }
           /* Buffer passed to EVP_EncryptFinal() must be after data just
            * encrypted to avoid overwriting it.
            */
           if(!EVP_EncryptFinal_ex(ctx, outbuf + outlen, &tmplen))
                   {
                   /* Error */
                   return 0;
                   }
           outlen += tmplen;
           EVP_CIPHER_CTX_free(ctx);
           /* Need binary mode for fopen because encrypted data is
            * binary data. Also cannot use strlen() on it because
            * it won't be null terminated and may contain embedded
            * nulls.
            */
           out = fopen(outfile, "wb");
           fwrite(outbuf, 1, outlen, out);
           fclose(out);
           return 1;
           }

The ciphertext from the above example can be decrypted using the **openssl**
utility with the command line (shown on two lines for clarity):

    openssl idea -d <filename
             -K 000102030405060708090A0B0C0D0E0F -iv 0102030405060708

General encryption and decryption function example using FILE I/O and AES128
with a 128-bit key:

    int do_crypt(FILE *in, FILE *out, int do_encrypt)
           {
           /* Allow enough space in output buffer for additional block */
           unsigned char inbuf[1024], outbuf[1024 + EVP_MAX_BLOCK_LENGTH];
           int inlen, outlen;
           EVP_CIPHER_CTX *ctx;
           /* Bogus key and IV: we'd normally set these from
            * another source.
            */
           unsigned char key[] = "0123456789abcdeF";
           unsigned char iv[] = "1234567887654321";

           /* Don't set key or IV right away; we want to check lengths */
           ctx = EVP_CIPHER_CTX_new();
           EVP_CipherInit_ex(&ctx, EVP_aes_128_cbc(), NULL, NULL, NULL,
                   do_encrypt);
           OPENSSL_assert(EVP_CIPHER_CTX_key_length(ctx) == 16);
           OPENSSL_assert(EVP_CIPHER_CTX_iv_length(ctx) == 16);

           /* Now we can set key and IV */
           EVP_CipherInit_ex(ctx, NULL, NULL, key, iv, do_encrypt);

           for(;;)
                   {
                   inlen = fread(inbuf, 1, 1024, in);
                   if (inlen <= 0) break;
                   if(!EVP_CipherUpdate(ctx, outbuf, &outlen, inbuf, inlen))
                           {
                           /* Error */
                           EVP_CIPHER_CTX_free(ctx);
                           return 0;
                           }
                   fwrite(outbuf, 1, outlen, out);
                   }
           if(!EVP_CipherFinal_ex(ctx, outbuf, &outlen))
                   {
                   /* Error */
                   EVP_CIPHER_CTX_free(ctx);
                   return 0;
                   }
           fwrite(outbuf, 1, outlen, out);

           EVP_CIPHER_CTX_free(ctx);
           return 1;
           }

# SEE ALSO

[evp(7)](http://man.he.net/man7/evp)

# HISTORY

Support for OCB mode was added in OpenSSL 1.1.0

**EVP\_CIPHER\_CTX** was made opaque in OpenSSL 1.1.0.  As a result,
EVP\_CIPHER\_CTX\_reset() appeared and EVP\_CIPHER\_CTX\_cleanup()
disappeared.  EVP\_CIPHER\_CTX\_init() remains as an alias for
EVP\_CIPHER\_CTX\_reset().

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
