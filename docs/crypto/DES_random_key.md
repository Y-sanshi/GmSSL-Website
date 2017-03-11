# NAME

DES\_random\_key, DES\_set\_key, DES\_key\_sched, DES\_set\_key\_checked,
DES\_set\_key\_unchecked, DES\_set\_odd\_parity, DES\_is\_weak\_key,
DES\_ecb\_encrypt, DES\_ecb2\_encrypt, DES\_ecb3\_encrypt, DES\_ncbc\_encrypt,
DES\_cfb\_encrypt, DES\_ofb\_encrypt, DES\_pcbc\_encrypt, DES\_cfb64\_encrypt,
DES\_ofb64\_encrypt, DES\_xcbc\_encrypt, DES\_ede2\_cbc\_encrypt,
DES\_ede2\_cfb64\_encrypt, DES\_ede2\_ofb64\_encrypt, DES\_ede3\_cbc\_encrypt,
DES\_ede3\_cfb64\_encrypt, DES\_ede3\_ofb64\_encrypt,
DES\_cbc\_cksum, DES\_quad\_cksum, DES\_string\_to\_key, DES\_string\_to\_2keys,
DES\_fcrypt, DES\_crypt - DES encryption

# SYNOPSIS

    #include <openssl/des.h>

    void DES_random_key(DES_cblock *ret);

    int DES_set_key(const_DES_cblock *key, DES_key_schedule *schedule);
    int DES_key_sched(const_DES_cblock *key, DES_key_schedule *schedule);
    int DES_set_key_checked(const_DES_cblock *key,
           DES_key_schedule *schedule);
    void DES_set_key_unchecked(const_DES_cblock *key,
           DES_key_schedule *schedule);

    void DES_set_odd_parity(DES_cblock *key);
    int DES_is_weak_key(const_DES_cblock *key);

    void DES_ecb_encrypt(const_DES_cblock *input, DES_cblock *output,
           DES_key_schedule *ks, int enc);
    void DES_ecb2_encrypt(const_DES_cblock *input, DES_cblock *output,
           DES_key_schedule *ks1, DES_key_schedule *ks2, int enc);
    void DES_ecb3_encrypt(const_DES_cblock *input, DES_cblock *output,
           DES_key_schedule *ks1, DES_key_schedule *ks2,
           DES_key_schedule *ks3, int enc);

    void DES_ncbc_encrypt(const unsigned char *input, unsigned char *output,
           long length, DES_key_schedule *schedule, DES_cblock *ivec,
           int enc);
    void DES_cfb_encrypt(const unsigned char *in, unsigned char *out,
           int numbits, long length, DES_key_schedule *schedule,
           DES_cblock *ivec, int enc);
    void DES_ofb_encrypt(const unsigned char *in, unsigned char *out,
           int numbits, long length, DES_key_schedule *schedule,
           DES_cblock *ivec);
    void DES_pcbc_encrypt(const unsigned char *input, unsigned char *output,
           long length, DES_key_schedule *schedule, DES_cblock *ivec,
           int enc);
    void DES_cfb64_encrypt(const unsigned char *in, unsigned char *out,
           long length, DES_key_schedule *schedule, DES_cblock *ivec,
           int *num, int enc);
    void DES_ofb64_encrypt(const unsigned char *in, unsigned char *out,
           long length, DES_key_schedule *schedule, DES_cblock *ivec,
           int *num);

    void DES_xcbc_encrypt(const unsigned char *input, unsigned char *output,
           long length, DES_key_schedule *schedule, DES_cblock *ivec,
           const_DES_cblock *inw, const_DES_cblock *outw, int enc);

    void DES_ede2_cbc_encrypt(const unsigned char *input,
           unsigned char *output, long length, DES_key_schedule *ks1,
           DES_key_schedule *ks2, DES_cblock *ivec, int enc);
    void DES_ede2_cfb64_encrypt(const unsigned char *in,
           unsigned char *out, long length, DES_key_schedule *ks1,
           DES_key_schedule *ks2, DES_cblock *ivec, int *num, int enc);
    void DES_ede2_ofb64_encrypt(const unsigned char *in,
           unsigned char *out, long length, DES_key_schedule *ks1,
           DES_key_schedule *ks2, DES_cblock *ivec, int *num);

    void DES_ede3_cbc_encrypt(const unsigned char *input,
           unsigned char *output, long length, DES_key_schedule *ks1,
           DES_key_schedule *ks2, DES_key_schedule *ks3, DES_cblock *ivec,
           int enc);
    void DES_ede3_cfb64_encrypt(const unsigned char *in, unsigned char *out,
           long length, DES_key_schedule *ks1, DES_key_schedule *ks2,
           DES_key_schedule *ks3, DES_cblock *ivec, int *num, int enc);
    void DES_ede3_ofb64_encrypt(const unsigned char *in, unsigned char *out,
           long length, DES_key_schedule *ks1,
           DES_key_schedule *ks2, DES_key_schedule *ks3,
           DES_cblock *ivec, int *num);

    DES_LONG DES_cbc_cksum(const unsigned char *input, DES_cblock *output,
           long length, DES_key_schedule *schedule,
           const_DES_cblock *ivec);
    DES_LONG DES_quad_cksum(const unsigned char *input, DES_cblock output[],
           long length, int out_count, DES_cblock *seed);
    void DES_string_to_key(const char *str, DES_cblock *key);
    void DES_string_to_2keys(const char *str, DES_cblock *key1,
           DES_cblock *key2);

    char *DES_fcrypt(const char *buf, const char *salt, char *ret);
    char *DES_crypt(const char *buf, const char *salt);

# DESCRIPTION

This library contains a fast implementation of the DES encryption
algorithm.

There are two phases to the use of DES encryption.  The first is the
generation of a _DES\_key\_schedule_ from a key, the second is the
actual encryption.  A DES key is of type _DES\_cblock_. This type is
consists of 8 bytes with odd parity.  The least significant bit in
each byte is the parity bit.  The key schedule is an expanded form of
the key; it is used to speed the encryption process.

DES\_random\_key() generates a random key.  The PRNG must be seeded
prior to using this function (see [rand(3)](http://man.he.net/man3/rand)).  If the PRNG
could not generate a secure key, 0 is returned.

Before a DES key can be used, it must be converted into the
architecture dependent _DES\_key\_schedule_ via the
DES\_set\_key\_checked() or DES\_set\_key\_unchecked() function.

DES\_set\_key\_checked() will check that the key passed is of odd parity
and is not a week or semi-weak key.  If the parity is wrong, then -1
is returned.  If the key is a weak key, then -2 is returned.  If an
error is returned, the key schedule is not generated.

DES\_set\_key() works like
DES\_set\_key\_checked() if the _DES\_check\_key_ flag is non-zero,
otherwise like DES\_set\_key\_unchecked().  These functions are available
for compatibility; it is recommended to use a function that does not
depend on a global variable.

DES\_set\_odd\_parity() sets the parity of the passed _key_ to odd.

DES\_is\_weak\_key() returns 1 if the passed key is a weak key, 0 if it
is ok.

The following routines mostly operate on an input and output stream of
_DES\_cblock_s.

DES\_ecb\_encrypt() is the basic DES encryption routine that encrypts or
decrypts a single 8-byte _DES\_cblock_ in _electronic code book_
(ECB) mode.  It always transforms the input data, pointed to by
_input_, into the output data, pointed to by the _output_ argument.
If the _encrypt_ argument is non-zero (DES\_ENCRYPT), the _input_
(cleartext) is encrypted in to the _output_ (ciphertext) using the
key\_schedule specified by the _schedule_ argument, previously set via
_DES\_set\_key_. If _encrypt_ is zero (DES\_DECRYPT), the _input_ (now
ciphertext) is decrypted into the _output_ (now cleartext).  Input
and output may overlap.  DES\_ecb\_encrypt() does not return a value.

DES\_ecb3\_encrypt() encrypts/decrypts the _input_ block by using
three-key Triple-DES encryption in ECB mode.  This involves encrypting
the input with _ks1_, decrypting with the key schedule _ks2_, and
then encrypting with _ks3_.  This routine greatly reduces the chances
of brute force breaking of DES and has the advantage of if _ks1_,
_ks2_ and _ks3_ are the same, it is equivalent to just encryption
using ECB mode and _ks1_ as the key.

The macro DES\_ecb2\_encrypt() is provided to perform two-key Triple-DES
encryption by using _ks1_ for the final encryption.

DES\_ncbc\_encrypt() encrypts/decrypts using the _cipher-block-chaining_
(CBC) mode of DES.  If the _encrypt_ argument is non-zero, the
routine cipher-block-chain encrypts the cleartext data pointed to by
the _input_ argument into the ciphertext pointed to by the _output_
argument, using the key schedule provided by the _schedule_ argument,
and initialization vector provided by the _ivec_ argument.  If the
_length_ argument is not an integral multiple of eight bytes, the
last block is copied to a temporary area and zero filled.  The output
is always an integral multiple of eight bytes.

DES\_xcbc\_encrypt() is RSA's DESX mode of DES.  It uses _inw_ and
_outw_ to 'whiten' the encryption.  _inw_ and _outw_ are secret
(unlike the iv) and are as such, part of the key.  So the key is sort
of 24 bytes.  This is much better than CBC DES.

DES\_ede3\_cbc\_encrypt() implements outer triple CBC DES encryption with
three keys. This means that each DES operation inside the CBC mode is
an `C=E(ks3,D(ks2,E(ks1,M)))`.  This mode is used by SSL.

The DES\_ede2\_cbc\_encrypt() macro implements two-key Triple-DES by
reusing _ks1_ for the final encryption.  `C=E(ks1,D(ks2,E(ks1,M)))`.
This form of Triple-DES is used by the RSAREF library.

DES\_pcbc\_encrypt() encrypt/decrypts using the propagating cipher block
chaining mode used by Kerberos v4. Its parameters are the same as
DES\_ncbc\_encrypt().

DES\_cfb\_encrypt() encrypt/decrypts using cipher feedback mode.  This
method takes an array of characters as input and outputs and array of
characters.  It does not require any padding to 8 character groups.
Note: the _ivec_ variable is changed and the new changed value needs to
be passed to the next call to this function.  Since this function runs
a complete DES ECB encryption per _numbits_, this function is only
suggested for use when sending small numbers of characters.

DES\_cfb64\_encrypt()
implements CFB mode of DES with 64bit feedback.  Why is this
useful you ask?  Because this routine will allow you to encrypt an
arbitrary number of bytes, no 8 byte padding.  Each call to this
routine will encrypt the input bytes to output and then update ivec
and num.  num contains 'how far' we are though ivec.  If this does
not make much sense, read more about cfb mode of DES :-).

DES\_ede3\_cfb64\_encrypt() and DES\_ede2\_cfb64\_encrypt() is the same as
DES\_cfb64\_encrypt() except that Triple-DES is used.

DES\_ofb\_encrypt() encrypts using output feedback mode.  This method
takes an array of characters as input and outputs and array of
characters.  It does not require any padding to 8 character groups.
Note: the _ivec_ variable is changed and the new changed value needs to
be passed to the next call to this function.  Since this function runs
a complete DES ECB encryption per numbits, this function is only
suggested for use when sending small numbers of characters.

DES\_ofb64\_encrypt() is the same as DES\_cfb64\_encrypt() using Output
Feed Back mode.

DES\_ede3\_ofb64\_encrypt() and DES\_ede2\_ofb64\_encrypt() is the same as
DES\_ofb64\_encrypt(), using Triple-DES.

The following functions are included in the DES library for
compatibility with the MIT Kerberos library.

DES\_cbc\_cksum() produces an 8 byte checksum based on the input stream
(via CBC encryption).  The last 4 bytes of the checksum are returned
and the complete 8 bytes are placed in _output_. This function is
used by Kerberos v4.  Other applications should use
[EVP\_DigestInit(3)](http://man.he.net/man3/EVP_DigestInit) etc. instead.

DES\_quad\_cksum() is a Kerberos v4 function.  It returns a 4 byte
checksum from the input bytes.  The algorithm can be iterated over the
input, depending on _out\_count_, 1, 2, 3 or 4 times.  If _output_ is
non-NULL, the 8 bytes generated by each pass are written into
_output_.

The following are DES-based transformations:

DES\_fcrypt() is a fast version of the Unix crypt(3) function.  This
version takes only a small amount of space relative to other fast
crypt() implementations.  This is different to the normal crypt in
that the third parameter is the buffer that the return value is
written into.  It needs to be at least 14 bytes long.  This function
is thread safe, unlike the normal crypt.

DES\_crypt() is a faster replacement for the normal system crypt().
This function calls DES\_fcrypt() with a static array passed as the
third parameter.  This mostly emulates the normal non-thread-safe semantics
of crypt(3).
The **salt** must be two ASCII characters.

DES\_enc\_write() writes _len_ bytes to file descriptor _fd_ from
buffer _buf_. The data is encrypted via _pcbc\_encrypt_ (default)
using _sched_ for the key and _iv_ as a starting vector.  The actual
data send down _fd_ consists of 4 bytes (in network byte order)
containing the length of the following encrypted data.  The encrypted
data then follows, padded with random data out to a multiple of 8
bytes.

# BUGS

DES\_3cbc\_encrypt() is flawed and must not be used in applications.

DES\_cbc\_encrypt() does not modify **ivec**; use DES\_ncbc\_encrypt()
instead.

DES\_cfb\_encrypt() and DES\_ofb\_encrypt() operates on input of 8 bits.
What this means is that if you set numbits to 12, and length to 2, the
first 12 bits will come from the 1st input byte and the low half of
the second input byte.  The second 12 bits will have the low 8 bits
taken from the 3rd input byte and the top 4 bits taken from the 4th
input byte.  The same holds for output.  This function has been
implemented this way because most people will be using a multiple of 8
and because once you get into pulling bytes input bytes apart things
get ugly!

DES\_string\_to\_key() is available for backward compatibility with the
MIT library.  New applications should use a cryptographic hash function.
The same applies for DES\_string\_to\_2key().

# NOTES

The **des** library was written to be source code compatible with
the MIT Kerberos library.

Applications should use the higher level functions
[EVP\_EncryptInit(3)](http://man.he.net/man3/EVP_EncryptInit) etc. instead of calling these
functions directly.

Single-key DES is insecure due to its short key size.  ECB mode is
not suitable for most applications; see [des\_modes(7)](http://man.he.net/man7/des_modes).

# HISTORY

The requirement that the **salt** parameter to DES\_crypt() and DES\_fcrypt()
be two ASCII characters was first enforced in
OpenSSL 1.1.0.  Previous versions tried to use the letter uppercase **A**
if both character were not present, and could crash when given non-ASCII
on some platforms.

# SEE ALSO

[des\_modes(7)](http://man.he.net/man7/des_modes),
[EVP\_EncryptInit(3)](http://man.he.net/man3/EVP_EncryptInit)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
