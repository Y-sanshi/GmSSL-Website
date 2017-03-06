# NAME

gmssl - GmSSL command line tool

# SYNOPSIS

**gmssl**
_command_
\[ _command\_opts_ \]
\[ _command\_args_ \]

**gmssl** **list** \[ **standard-commands** | **digest-commands** | **cipher-commands** | **cipher-algorithms** | **digest-algorithms** | **public-key-algorithms**\]

**gmssl** **no-**_XXX_ \[ _arbitrary options_ \]

# DESCRIPTION

GmSSL is a cryptography toolkit implementing the Secure Sockets Layer (SSL
v2/v3) and Transport Layer Security (TLS v1) network protocols and related
cryptography standards required by them.

The **gmssl** program is a command line tool for using the various
cryptography functions of GmSSL's **crypto** library from the shell.
It can be used for

    o  Creation and management of private keys, public keys and parameters
    o  Public key cryptographic operations
    o  Creation of X.509 certificates, CSRs and CRLs
    o  Calculation of Message Digests
    o  Encryption and Decryption with Ciphers
    o  SSL/TLS Client and Server Tests
    o  Handling of S/MIME signed or encrypted mail
    o  Time Stamp requests, generation and verification

# COMMAND SUMMARY

The **gmssl** program provides a rich variety of commands (_command_ in the
SYNOPSIS above), each of which often has a wealth of options and arguments
(_command\_opts_ and _command\_args_ in the SYNOPSIS).

The list parameters **standard-commands**, **digest-commands**,
and **cipher-commands** output a list (one entry per line) of the names
of all standard commands, message digest commands, or cipher commands,
respectively, that are available in the present **gmssl** utility.

The list parameters **cipher-algorithms** and
**digest-algorithms** list all cipher and message digest names, one entry per line. Aliases are listed as:

    from => to

The list parameter **public-key-algorithms** lists all supported public
key algorithms.

The command **no-**_XXX_ tests whether a command of the
specified name is available.  If no command named _XXX_ exists, it
returns 0 (success) and prints **no-**_XXX_; otherwise it returns 1
and prints _XXX_.  In both cases, the output goes to **stdout** and
nothing is printed to **stderr**.  Additional command line arguments
are always ignored.  Since for each cipher there is a command of the
same name, this provides an easy way for shell scripts to test for the
availability of ciphers in the **gmssl** program.  (**no-**_XXX_ is
not able to detect pseudo-commands such as **quit**,
**list**, or **no-**_XXX_ itself.)

## Standard Commands

- [**asn1parse**](http://man.he.net/man1/asn1parse)

    Parse an ASN.1 sequence.

- [**ca**](http://man.he.net/man1/ca)

    Certificate Authority (CA) Management.

- [**ciphers**](http://man.he.net/man1/ciphers)

    Cipher Suite Description Determination.

- [**cms**](http://man.he.net/man1/cms)

    CMS (Cryptographic Message Syntax) utility

- [**crl**](http://man.he.net/man1/crl)

    Certificate Revocation List (CRL) Management.

- [**crl2pkcs7**](http://man.he.net/man1/crl2pkcs7)

    CRL to PKCS#7 Conversion.

- [**dgst**](http://man.he.net/man1/dgst)

    Message Digest Calculation.

- **dh**

    Diffie-Hellman Parameter Management.
    Obsoleted by [**dhparam**](http://man.he.net/man1/dhparam).

- [**dhparam**](http://man.he.net/man1/dhparam)

    Generation and Management of Diffie-Hellman Parameters. Superseded by
    [**genpkey**](http://man.he.net/man1/genpkey) and [**pkeyparam**](http://man.he.net/man1/pkeyparam)

- [**dsa**](http://man.he.net/man1/dsa)

    DSA Data Management.

- [**dsaparam**](http://man.he.net/man1/dsaparam)

    DSA Parameter Generation and Management. Superseded by
    [**genpkey**](http://man.he.net/man1/genpkey) and [**pkeyparam**](http://man.he.net/man1/pkeyparam)

- [**ec**](http://man.he.net/man1/ec)

    EC/SM2 (Elliptic curve) key processing

- [**ecparam**](http://man.he.net/man1/ecparam)

    EC/SM2 parameter manipulation and generation

- [**enc**](http://man.he.net/man1/enc)

    Encoding with Ciphers.

- [**engine**](http://man.he.net/man1/engine)

    Engine (loadable module) information and manipulation.

- [**errstr**](http://man.he.net/man1/errstr)

    Error Number to Error String Conversion.

- **gendh**

    Generation of Diffie-Hellman Parameters.
    Obsoleted by [**dhparam**](http://man.he.net/man1/dhparam).

- [**gendsa**](http://man.he.net/man1/gendsa)

    Generation of DSA Private Key from Parameters. Superseded by
    [**genpkey**](http://man.he.net/man1/genpkey) and [**pkey**](http://man.he.net/man1/pkey)

- [**genpkey**](http://man.he.net/man1/genpkey)

    Generation of Private Key or Parameters.

- [**genrsa**](http://man.he.net/man1/genrsa)

    Generation of RSA Private Key. Superseded by [**genpkey**](http://man.he.net/man1/genpkey).

- [**nseq**](http://man.he.net/man1/nseq)

    Create or examine a Netscape certificate sequence

- [**ocsp**](http://man.he.net/man1/ocsp)

    Online Certificate Status Protocol utility.

- [**passwd**](http://man.he.net/man1/passwd)

    Generation of hashed passwords.

- [**pkcs12**](http://man.he.net/man1/pkcs12)

    PKCS#12 Data Management.

- [**pkcs7**](http://man.he.net/man1/pkcs7)

    PKCS#7 Data Management.

- [**pkey**](http://man.he.net/man1/pkey)

    Public and private key management.

- [**pkeyparam**](http://man.he.net/man1/pkeyparam)

    Public key algorithm parameter management.

- [**pkeyutl**](http://man.he.net/man1/pkeyutl)

    Public key algorithm cryptographic operation utility.

- [**rand**](http://man.he.net/man1/rand)

    Generate pseudo-random bytes.

- [**req**](http://man.he.net/man1/req)

    PKCS#10 X.509 Certificate Signing Request (CSR) Management.

- [**rsa**](http://man.he.net/man1/rsa)

    RSA key management.

- [**rsautl**](http://man.he.net/man1/rsautl)

    RSA utility for signing, verification, encryption, and decryption. Superseded
    by  [**pkeyutl**](http://man.he.net/man1/pkeyutl)

- [**s\_client**](http://man.he.net/man1/s_client)

    This implements a generic SSL/TLS client which can establish a transparent
    connection to a remote server speaking SSL/TLS. It's intended for testing
    purposes only and provides only rudimentary interface functionality but
    internally uses mostly all functionality of the GmSSL **ssl** library.

- [**s\_server**](http://man.he.net/man1/s_server)

    This implements a generic SSL/TLS server which accepts connections from remote
    clients speaking SSL/TLS. It's intended for testing purposes only and provides
    only rudimentary interface functionality but internally uses mostly all
    functionality of the GmSSL **ssl** library.  It provides both an own command
    line oriented protocol for testing SSL functions and a simple HTTP response
    facility to emulate an SSL/TLS-aware webserver.

- [**s\_time**](http://man.he.net/man1/s_time)

    SSL Connection Timer.

- [**sess\_id**](http://man.he.net/man1/sess_id)

    SSL Session Data Management.

- [**smime**](http://man.he.net/man1/smime)

    S/MIME mail processing.

- [**speed**](http://man.he.net/man1/speed)

    Algorithm Speed Measurement.

- [**spkac**](http://man.he.net/man1/spkac)

    SPKAC printing and generating utility

- [**ts**](http://man.he.net/man1/ts)

    Time Stamping Authority tool (client/server)

- [**verify**](http://man.he.net/man1/verify)

    X.509 Certificate Verification.

- [**version**](http://man.he.net/man1/version)

    GmSSL Version Information.

- [**x509**](http://man.he.net/man1/x509)

    X.509 Certificate Data Management.

## Message Digest Commands

- **sm3**

    SM3 Digest

- **md5**

    MD5 Digest

- **mdc2**

    MDC2 Digest

- **rmd160**

    RMD-160 Digest

- **sha**

    SHA Digest

- **sha1**

    SHA-1 Digest

- **sha224**

    SHA-224 Digest

- **sha256**

    SHA-256 Digest

- **sha384**

    SHA-384 Digest

- **sha512**

    SHA-512 Digest

## Encoding and Cipher Commands

- **base64**

    Base64 Encoding

- **sms4 sms4-cbc sms4-cfb sms4-ecb sms4-ofb**

    SMS4 Cipher

- **cast cast-cbc**

    CAST Cipher

- **cast5-cbc cast5-cfb cast5-ecb cast5-ofb**

    CAST5 Cipher

- **des des-cbc des-cfb des-ecb des-ede des-ede-cbc des-ede-cfb des-ede-ofb des-ofb**

    DES Cipher

- **des3 desx des-ede3 des-ede3-cbc des-ede3-cfb des-ede3-ofb**

    Triple-DES Cipher

- **idea idea-cbc idea-cfb idea-ecb idea-ofb**

    IDEA Cipher

- **rc2 rc2-cbc rc2-cfb rc2-ecb rc2-ofb**

    RC2 Cipher

- **rc4**

    RC4 Cipher

- **rc5 rc5-cbc rc5-cfb rc5-ecb rc5-ofb**

    RC5 Cipher

# OPTIONS

Details of which options are available depend on the specific command.
This section describes some common options with common behavior.

## Common Options

- **-help**

    Provides a terse summary of all options.

## Pass Phrase Options

Several commands accept password arguments, typically using **-passin**
and **-passout** for input and output passwords respectively. These allow
the password to be obtained from a variety of sources. Both of these
options take a single argument whose format is described below. If no
password argument is given and a password is required then the user is
prompted to enter one: this will typically be read from the current
terminal with echoing turned off.

- **pass:password**

    the actual password is **password**. Since the password is visible
    to utilities (like 'ps' under Unix) this form should only be used
    where security is not important.

- **env:var**

    obtain the password from the environment variable **var**. Since
    the environment of other processes is visible on certain platforms
    (e.g. ps under certain Unix OSes) this option should be used with caution.

- **file:pathname**

    the first line of **pathname** is the password. If the same **pathname**
    argument is supplied to **-passin** and **-passout** arguments then the first
    line will be used for the input password and the next line for the output
    password. **pathname** need not refer to a regular file: it could for example
    refer to a device or named pipe.

- **fd:number**

    read the password from the file descriptor **number**. This can be used to
    send the data via a pipe for example.

- **stdin**

    read the password from standard input.

# SEE ALSO

[asn1parse(1)](http://man.he.net/man1/asn1parse), [ca(1)](http://man.he.net/man1/ca), [config(5)](http://man.he.net/man5/config),
[crl(1)](http://man.he.net/man1/crl), [crl2pkcs7(1)](http://man.he.net/man1/crl2pkcs7), [dgst(1)](http://man.he.net/man1/dgst),
[dhparam(1)](http://man.he.net/man1/dhparam), [dsa(1)](http://man.he.net/man1/dsa), [dsaparam(1)](http://man.he.net/man1/dsaparam),
[enc(1)](http://man.he.net/man1/enc), [engine(1)](http://man.he.net/man1/engine), [gendsa(1)](http://man.he.net/man1/gendsa), [genpkey(1)](http://man.he.net/man1/genpkey),
[genrsa(1)](http://man.he.net/man1/genrsa), [nseq(1)](http://man.he.net/man1/nseq), [gmssl(1)](http://man.he.net/man1/gmssl),
[passwd(1)](http://man.he.net/man1/passwd),
[pkcs12(1)](http://man.he.net/man1/pkcs12), [pkcs7(1)](http://man.he.net/man1/pkcs7), [pkcs8(1)](http://man.he.net/man1/pkcs8),
[rand(1)](http://man.he.net/man1/rand), [req(1)](http://man.he.net/man1/req), [rsa(1)](http://man.he.net/man1/rsa),
[rsautl(1)](http://man.he.net/man1/rsautl), [s\_client(1)](http://man.he.net/man1/s_client),
[s\_server(1)](http://man.he.net/man1/s_server), [s\_time(1)](http://man.he.net/man1/s_time),
[smime(1)](http://man.he.net/man1/smime), [spkac(1)](http://man.he.net/man1/spkac),
[verify(1)](http://man.he.net/man1/verify), [version(1)](http://man.he.net/man1/version), [x509(1)](http://man.he.net/man1/x509),
[crypto(7)](http://man.he.net/man7/crypto), [ssl(7)](http://man.he.net/man7/ssl), [x509v3\_config(5)](http://man.he.net/man5/x509v3_config)

# HISTORY

The **list-**_XXX_**-algorithms** pseudo-commands were added in GmSSL 1.0.0;
For notes on the availability of other commands, see their individual
manual pages.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the GmSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
