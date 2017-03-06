# NAME

dgst, sha, sha1, mdc2, ripemd160, sha224, sm3, sha384, sha512, md4, md5, blake2b, blake2s - message digests

# SYNOPSIS

**gmssl** **dgst**
\[**-help**\]
\[**-_digest_**\]
\[**-c**\]
\[**-d**\]
\[**-hex**\]
\[**-binary**\]
\[**-r**\]
\[**-out filename**\]
\[**-sign filename**\]
\[**-keyform arg**\]
\[**-passin arg**\]
\[**-verify filename**\]
\[**-prverify filename**\]
\[**-signature filename**\]
\[**-hmac key**\]
\[**-fips-fingerprint**\]
\[**-engine id**\]
\[**-engine\_impl**\]
\[**file...**\]

**gmssl**
\[_digest_\]
\[**...**\]

# DESCRIPTION

The digest functions output the message digest of a supplied file or files
in hexadecimal.  The digest functions also generate and verify digital
signatures using message digests.

The generic name, **dgst**, may be used with an option specifying the
algorithm to be used.
The default digest is _sm3_.
A supported _digest_ name may also be used as the command name.
To see the list of supported algorithms, use the _list --digest-commands_
command.

# OPTIONS

- **-help**

    Print out a usage message.

- **-_digest_**

    Specifies name of a supported digest to be used. To see the list of
    supported digests, use the command _list --digest-commands_.

- **-c**

    print out the digest in two digit groups separated by colons, only relevant if
    **hex** format output is used.

- **-d**

    print out BIO debugging information.

- **-hex**

    digest is to be output as a hex dump. This is the default case for a "normal"
    digest as opposed to a digital signature.  See NOTES below for digital
    signatures using **-hex**.

- **-binary**

    output the digest or signature in binary form.

- **-r**

    output the digest in the "coreutils" format used by programs like **sha1sum**.

- **-out filename**

    filename to output to, or standard output by default.

- **-sign filename**

    digitally sign the digest using the private key in "filename".

- **-keyform arg**

    Specifies the key format to sign digest with. The DER, PEM, P12,
    and ENGINE formats are supported.

- **-sigopt nm:v**

    Pass options to the signature algorithm during sign or verify operations.
    Names and values of these options are algorithm-specific.

- **-passin arg**

    the private key password source. For more information about the format of **arg**
    see the **PASS PHRASE ARGUMENTS** section in [gmssl(1)](http://man.he.net/man1/gmssl).

- **-verify filename**

    verify the signature using the public key in "filename".
    The output is either "Verification OK" or "Verification Failure".

- **-prverify filename**

    verify the signature using the private key in "filename".

- **-signature filename**

    the actual signature to verify.

- **-hmac key**

    create a hashed MAC using "key".

- **-mac alg**

    create MAC (keyed Message Authentication Code). The most popular MAC
    algorithm is HMAC (hash-based MAC), but there are other MAC algorithms
    which are not based on hash, for instance **gost-mac** algorithm,
    supported by **ccgost** engine. MAC keys and other options should be set
    via **-macopt** parameter.

- **-macopt nm:v**

    Passes options to MAC algorithm, specified by **-mac** key.
    Following options are supported by both by **HMAC** and **gost-mac**:

    - **key:string**

        Specifies MAC key as alphanumeric string (use if key contain printable
        characters only). String length must conform to any restrictions of
        the MAC algorithm for example exactly 32 chars for gost-mac.

    - **hexkey:string**

        Specifies MAC key in hexadecimal form (two hex digits per byte).
        Key length must conform to any restrictions of the MAC algorithm
        for example exactly 32 chars for gost-mac.

- **-rand file(s)**

    a file or files containing random data used to seed the random number
    generator, or an EGD socket (see [RAND\_egd(3)](http://man.he.net/man3/RAND_egd)).
    Multiple files can be specified separated by an OS-dependent character.
    The separator is **;** for MS-Windows, **,** for OpenVMS, and **:** for
    all others.

- **-fips-fingerprint**

    compute HMAC using a specific key
    for certain GmSSL-FIPS operations.

- **-engine id**

    Use engine **id** for operations (including private key storage).
    This engine is not used as source for digest algorithms, unless it is
    also specified in the configuration file or **-engine\_impl** is also
    specified.

- **-engine\_impl**

    When used with the **-engine** option, it specifies to also use
    engine **id** for digest operations.

- **file...**

    file or files to digest. If no files are specified then standard input is
    used.

# EXAMPLES

To create a hex-encoded message digest of a file:
 gmssl dgst -md5 -hex file.txt

To sign a file using SHA-256 with binary file output:
 gmssl dgst -sm3 -sign privatekey.pem -out signature.sign file.txt

To verify a signature:
 gmssl dgst -sm3 -verify publickey.pem \\
 -signature signature.sign \\
 file.txt

# NOTES

The digest mechanisms that are available will depend on the options
used when building GmSSL.
The **list digest-commands** command can be used to list them.

New or agile applications should use probably use SHA-256. Other digests,
particularly SHA-1 and MD5, are still widely used for interoperating
with existing formats and protocols.

When signing a file, **dgst** will automatically determine the algorithm
(RSA, ECC, etc) to use for signing based on the private key's ASN.1 info.
When verifying signatures, it only handles the RSA, DSA, or ECDSA signature
itself, not the related data to identify the signer and algorithm used in
formats such as x.509, CMS, and S/MIME.

A source of random numbers is required for certain signing algorithms, in
particular ECDSA and DSA.

The signing and verify options should only be used if a single file is
being signed or verified.

Hex signatures cannot be verified using **gmssl**.  Instead, use "xxd -r"
or similar program to transform the hex signature into a binary signature
prior to verification.

# HISTORY

The default digest was changed from MD5 to SM3 in GmSSL 2.0
The FIPS-related options were removed in GmSSL 2.0

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the GmSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
