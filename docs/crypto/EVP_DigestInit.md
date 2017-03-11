# NAME

EVP\_MD\_CTX\_new, EVP\_MD\_CTX\_reset, EVP\_MD\_CTX\_free, EVP\_MD\_CTX\_copy\_ex,
EVP\_DigestInit\_ex, EVP\_DigestUpdate, EVP\_DigestFinal\_ex,
EVP\_DigestInit, EVP\_DigestFinal, EVP\_MD\_CTX\_copy, EVP\_MD\_type,
EVP\_MD\_pkey\_type, EVP\_MD\_size, EVP\_MD\_block\_size, EVP\_MD\_CTX\_md, EVP\_MD\_CTX\_size,
EVP\_MD\_CTX\_block\_size, EVP\_MD\_CTX\_type, EVP\_md\_null, EVP\_md2, EVP\_md5, EVP\_sha1,
EVP\_sha224, EVP\_sha256, EVP\_sha384, EVP\_sha512, EVP\_mdc2,
EVP\_ripemd160, EVP\_blake2b512, EVP\_blake2s256, EVP\_get\_digestbyname,
EVP\_get\_digestbynid, EVP\_get\_digestbyobj - EVP digest routines

# SYNOPSIS

    #include <openssl/evp.h>

    EVP_MD_CTX *EVP_MD_CTX_new(void);
    int EVP_MD_CTX_reset(EVP_MD_CTX *ctx);
    void EVP_MD_CTX_free(EVP_MD_CTX *ctx);

    int EVP_DigestInit_ex(EVP_MD_CTX *ctx, const EVP_MD *type, ENGINE *impl);
    int EVP_DigestUpdate(EVP_MD_CTX *ctx, const void *d, size_t cnt);
    int EVP_DigestFinal_ex(EVP_MD_CTX *ctx, unsigned char *md,
           unsigned int *s);

    int EVP_MD_CTX_copy_ex(EVP_MD_CTX *out, const EVP_MD_CTX *in);

    int EVP_DigestInit(EVP_MD_CTX *ctx, const EVP_MD *type);
    int EVP_DigestFinal(EVP_MD_CTX *ctx, unsigned char *md,
           unsigned int *s);

    int EVP_MD_CTX_copy(EVP_MD_CTX *out, EVP_MD_CTX *in);

    int EVP_MD_type(const EVP_MD *md);
    int EVP_MD_pkey_type(const EVP_MD *md);
    int EVP_MD_size(const EVP_MD *md);
    int EVP_MD_block_size(const EVP_MD *md);

    const EVP_MD *EVP_MD_CTX_md(const EVP_MD_CTX *ctx);
    int EVP_MD_CTX_size(const EVP_MD *ctx);
    int EVP_MD_CTX_block_size(const EVP_MD *ctx);
    int EVP_MD_CTX_type(const EVP_MD *ctx);

    const EVP_MD *EVP_md_null(void);
    const EVP_MD *EVP_md2(void);
    const EVP_MD *EVP_md5(void);
    const EVP_MD *EVP_sha1(void);
    const EVP_MD *EVP_mdc2(void);
    const EVP_MD *EVP_ripemd160(void);
    const EVP_MD *EVP_blake2b512(void);
    const EVP_MD *EVP_blake2s256(void);

    const EVP_MD *EVP_sha224(void);
    const EVP_MD *EVP_sha256(void);
    const EVP_MD *EVP_sha384(void);
    const EVP_MD *EVP_sha512(void);

    const EVP_MD *EVP_get_digestbyname(const char *name);
    const EVP_MD *EVP_get_digestbynid(int type);
    const EVP_MD *EVP_get_digestbyobj(const ASN1_OBJECT *o);

# DESCRIPTION

The EVP digest routines are a high level interface to message digests,
and should be used instead of the cipher-specific functions.

EVP\_MD\_CTX\_new() allocates, initializes and returns a digest context.

EVP\_MD\_CTX\_reset() resets the digest context **ctx**.  This can be used
to reuse an already existing context.

EVP\_MD\_CTX\_free() cleans up digest context **ctx** and frees up the
space allocated to it.

EVP\_DigestInit\_ex() sets up digest context **ctx** to use a digest
**type** from ENGINE **impl**. **ctx** must be initialized before calling this
function. **type** will typically be supplied by a function such as EVP\_sha1().
If **impl** is NULL then the default implementation of digest **type** is used.

EVP\_DigestUpdate() hashes **cnt** bytes of data at **d** into the
digest context **ctx**. This function can be called several times on the
same **ctx** to hash additional data.

EVP\_DigestFinal\_ex() retrieves the digest value from **ctx** and places
it in **md**. If the **s** parameter is not NULL then the number of
bytes of data written (i.e. the length of the digest) will be written
to the integer at **s**, at most **EVP\_MAX\_MD\_SIZE** bytes will be written.
After calling EVP\_DigestFinal\_ex() no additional calls to EVP\_DigestUpdate()
can be made, but EVP\_DigestInit\_ex() can be called to initialize a new
digest operation.

EVP\_MD\_CTX\_copy\_ex() can be used to copy the message digest state from
**in** to **out**. This is useful if large amounts of data are to be
hashed which only differ in the last few bytes. **out** must be initialized
before calling this function.

EVP\_DigestInit() behaves in the same way as EVP\_DigestInit\_ex() except
the passed context **ctx** does not have to be initialized, and it always
uses the default digest implementation.

EVP\_DigestFinal() is similar to EVP\_DigestFinal\_ex() except the digest
context **ctx** is automatically cleaned up.

EVP\_MD\_CTX\_copy() is similar to EVP\_MD\_CTX\_copy\_ex() except the destination
**out** does not have to be initialized.

EVP\_MD\_size() and EVP\_MD\_CTX\_size() return the size of the message digest
when passed an **EVP\_MD** or an **EVP\_MD\_CTX** structure, i.e. the size of the
hash.

EVP\_MD\_block\_size() and EVP\_MD\_CTX\_block\_size() return the block size of the
message digest when passed an **EVP\_MD** or an **EVP\_MD\_CTX** structure.

EVP\_MD\_type() and EVP\_MD\_CTX\_type() return the NID of the OBJECT IDENTIFIER
representing the given message digest when passed an **EVP\_MD** structure.
For example EVP\_MD\_type(EVP\_sha1()) returns **NID\_sha1**. This function is
normally used when setting ASN1 OIDs.

EVP\_MD\_CTX\_md() returns the **EVP\_MD** structure corresponding to the passed
**EVP\_MD\_CTX**.

EVP\_MD\_pkey\_type() returns the NID of the public key signing algorithm associated
with this digest. For example EVP\_sha1() is associated with RSA so this will
return **NID\_sha1WithRSAEncryption**. Since digests and signature algorithms
are no longer linked this function is only retained for compatibility
reasons.

EVP\_md2(), EVP\_md5(), EVP\_sha1(), EVP\_sha224(), EVP\_sha256(),
EVP\_sha384(), EVP\_sha512(), EVP\_mdc2(), EVP\_ripemd160(), EVP\_blake2b512(), and
EVP\_blake2s256() return **EVP\_MD** structures for the MD2, MD5, SHA1, SHA224,
SHA256, SHA384, SHA512, MDC2, RIPEMD160, BLAKE2b-512, and BLAKE2s-256 digest
algorithms respectively.

EVP\_md\_null() is a "null" message digest that does nothing: i.e. the hash it
returns is of zero length.

EVP\_get\_digestbyname(), EVP\_get\_digestbynid() and EVP\_get\_digestbyobj()
return an **EVP\_MD** structure when passed a digest name, a digest NID or
an ASN1\_OBJECT structure respectively.

# RETURN VALUES

EVP\_DigestInit\_ex(), EVP\_DigestUpdate() and EVP\_DigestFinal\_ex() return 1 for
success and 0 for failure.

EVP\_MD\_CTX\_copy\_ex() returns 1 if successful or 0 for failure.

EVP\_MD\_type(), EVP\_MD\_pkey\_type() and EVP\_MD\_type() return the NID of the
corresponding OBJECT IDENTIFIER or NID\_undef if none exists.

EVP\_MD\_size(), EVP\_MD\_block\_size(), EVP\_MD\_CTX\_size() and
EVP\_MD\_CTX\_block\_size() return the digest or block size in bytes.

EVP\_md\_null(), EVP\_md2(), EVP\_md5(), EVP\_sha1(),
EVP\_mdc2(), EVP\_ripemd160(), EVP\_blake2b512(), and EVP\_blake2s256() return
pointers to the corresponding EVP\_MD structures.

EVP\_get\_digestbyname(), EVP\_get\_digestbynid() and EVP\_get\_digestbyobj()
return either an **EVP\_MD** structure or NULL if an error occurs.

# NOTES

The **EVP** interface to message digests should almost always be used in
preference to the low level interfaces. This is because the code then becomes
transparent to the digest used and much more flexible.

New applications should use the SHA2 digest algorithms such as SHA256.
The other digest algorithms are still in common use.

For most applications the **impl** parameter to EVP\_DigestInit\_ex() will be
set to NULL to use the default digest implementation.

The functions EVP\_DigestInit(), EVP\_DigestFinal() and EVP\_MD\_CTX\_copy() are
obsolete but are retained to maintain compatibility with existing code. New
applications should use EVP\_DigestInit\_ex(), EVP\_DigestFinal\_ex() and
EVP\_MD\_CTX\_copy\_ex() because they can efficiently reuse a digest context
instead of initializing and cleaning it up on each call and allow non default
implementations of digests to be specified.

If digest contexts are not cleaned up after use
memory leaks will occur.

EVP\_MD\_CTX\_size(), EVP\_MD\_CTX\_block\_size(), EVP\_MD\_CTX\_type(),
EVP\_get\_digestbynid() and EVP\_get\_digestbyobj() are defined as
macros.

# EXAMPLE

This example digests the data "Test Message\\n" and "Hello World\\n", using the
digest name passed on the command line.

    #include <stdio.h>
    #include <openssl/evp.h>

    main(int argc, char *argv[])
    {
    EVP_MD_CTX *mdctx;
    const EVP_MD *md;
    char mess1[] = "Test Message\n";
    char mess2[] = "Hello World\n";
    unsigned char md_value[EVP_MAX_MD_SIZE];
    int md_len, i;

    if(!argv[1]) {
           printf("Usage: mdtest digestname\n");
           exit(1);
    }

    md = EVP_get_digestbyname(argv[1]);

    if(!md) {
           printf("Unknown message digest %s\n", argv[1]);
           exit(1);
    }

    mdctx = EVP_MD_CTX_new();
    EVP_DigestInit_ex(mdctx, md, NULL);
    EVP_DigestUpdate(mdctx, mess1, strlen(mess1));
    EVP_DigestUpdate(mdctx, mess2, strlen(mess2));
    EVP_DigestFinal_ex(mdctx, md_value, &md_len);
    EVP_MD_CTX_free(mdctx);

    printf("Digest is: ");
    for (i = 0; i < md_len; i++)
           printf("%02x", md_value[i]);
    printf("\n");

    exit(0);
    }

# SEE ALSO

[dgst(1)](http://man.he.net/man1/dgst),
[evp(7)](http://man.he.net/man7/evp)

# HISTORY

**EVP\_MD\_CTX** became opaque in OpenSSL 1.1.  Consequently, stack
allocated **EVP\_MD\_CTX**s are no longer supported.

EVP\_MD\_CTX\_create() and EVP\_MD\_CTX\_destroy() were renamed to
EVP\_MD\_CTX\_new() and EVP\_MD\_CTX\_free() in OpenSSL 1.1.

The link between digests and signing algorithms was fixed in OpenSSL 1.0 and
later, so now EVP\_sha1() can be used with RSA and DSA. The legacy EVP\_dss1()
was removed in OpenSSL 1.1.0

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
