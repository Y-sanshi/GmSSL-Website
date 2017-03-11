# NAME

BIO\_f\_md, BIO\_set\_md, BIO\_get\_md, BIO\_get\_md\_ctx - message digest BIO filter

# SYNOPSIS

    #include <openssl/bio.h>
    #include <openssl/evp.h>

    const BIO_METHOD *BIO_f_md(void);
    int BIO_set_md(BIO *b, EVP_MD *md);
    int BIO_get_md(BIO *b, EVP_MD **mdp);
    int BIO_get_md_ctx(BIO *b, EVP_MD_CTX **mdcp);

# DESCRIPTION

BIO\_f\_md() returns the message digest BIO method. This is a filter
BIO that digests any data passed through it, it is a BIO wrapper
for the digest routines EVP\_DigestInit(), EVP\_DigestUpdate()
and EVP\_DigestFinal().

Any data written or read through a digest BIO using BIO\_read() and
BIO\_write() is digested.

BIO\_gets(), if its **size** parameter is large enough finishes the
digest calculation and returns the digest value. BIO\_puts() is
not supported.

BIO\_reset() reinitialises a digest BIO.

BIO\_set\_md() sets the message digest of BIO **b** to **md**: this
must be called to initialize a digest BIO before any data is
passed through it. It is a BIO\_ctrl() macro.

BIO\_get\_md() places the a pointer to the digest BIOs digest method
in **mdp**, it is a BIO\_ctrl() macro.

BIO\_get\_md\_ctx() returns the digest BIOs context into **mdcp**.

# NOTES

The context returned by BIO\_get\_md\_ctx() can be used in calls
to EVP\_DigestFinal() and also the signature routines EVP\_SignFinal()
and EVP\_VerifyFinal().

The context returned by BIO\_get\_md\_ctx() is an internal context
structure. Changes made to this context will affect the digest
BIO itself and the context pointer will become invalid when the digest
BIO is freed.

After the digest has been retrieved from a digest BIO it must be
reinitialized by calling BIO\_reset(), or BIO\_set\_md() before any more
data is passed through it.

If an application needs to call BIO\_gets() or BIO\_puts() through
a chain containing digest BIOs then this can be done by prepending
a buffering BIO.

Calling BIO\_get\_md\_ctx() will return the context and initialize the BIO
state. This allows applications to initialize the context externally
if the standard calls such as BIO\_set\_md() are not sufficiently flexible.

# RETURN VALUES

BIO\_f\_md() returns the digest BIO method.

BIO\_set\_md(), BIO\_get\_md() and BIO\_md\_ctx() return 1 for success and
0 for failure.

# EXAMPLES

The following example creates a BIO chain containing an SHA1 and MD5
digest BIO and passes the string "Hello World" through it. Error
checking has been omitted for clarity.

    BIO *bio, *mdtmp;
    char message[] = "Hello World";
    bio = BIO_new(BIO_s_null());
    mdtmp = BIO_new(BIO_f_md());
    BIO_set_md(mdtmp, EVP_sha1());
    /* For BIO_push() we want to append the sink BIO and keep a note of
     * the start of the chain.
     */
    bio = BIO_push(mdtmp, bio);
    mdtmp = BIO_new(BIO_f_md());
    BIO_set_md(mdtmp, EVP_md5());
    bio = BIO_push(mdtmp, bio);
    /* Note: mdtmp can now be discarded */
    BIO_write(bio, message, strlen(message));

The next example digests data by reading through a chain instead:

    BIO *bio, *mdtmp;
    char buf[1024];
    int rdlen;
    bio = BIO_new_file(file, "rb");
    mdtmp = BIO_new(BIO_f_md());
    BIO_set_md(mdtmp, EVP_sha1());
    bio = BIO_push(mdtmp, bio);
    mdtmp = BIO_new(BIO_f_md());
    BIO_set_md(mdtmp, EVP_md5());
    bio = BIO_push(mdtmp, bio);
    do {
           rdlen = BIO_read(bio, buf, sizeof(buf));
           /* Might want to do something with the data here */
    } while (rdlen > 0);

This next example retrieves the message digests from a BIO chain and
outputs them. This could be used with the examples above.

    BIO *mdtmp;
    unsigned char mdbuf[EVP_MAX_MD_SIZE];
    int mdlen;
    int i;
    mdtmp = bio;   /* Assume bio has previously been set up */
    do {
           EVP_MD *md;
           mdtmp = BIO_find_type(mdtmp, BIO_TYPE_MD);
           if (!mdtmp) break;
           BIO_get_md(mdtmp, &md);
           printf("%s digest", OBJ_nid2sn(EVP_MD_type(md)));
           mdlen = BIO_gets(mdtmp, mdbuf, EVP_MAX_MD_SIZE);
           for (i = 0; i < mdlen; i++) printf(":%02X", mdbuf[i]);
           printf("\n");
           mdtmp = BIO_next(mdtmp);
    } while (mdtmp);

    BIO_free_all(bio);

# BUGS

The lack of support for BIO\_puts() and the non standard behaviour of
BIO\_gets() could be regarded as anomalous. It could be argued that BIO\_gets()
and BIO\_puts() should be passed to the next BIO in the chain and digest
the data passed through and that digests should be retrieved using a
separate BIO\_ctrl() call.

# HISTORY

Before OpenSSL 1.0.0., the call to BIO\_get\_md\_ctx() would only work if the
BIO was initialized first.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
