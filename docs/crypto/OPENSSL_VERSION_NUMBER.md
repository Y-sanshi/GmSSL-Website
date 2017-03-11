# NAME

OPENSSL\_VERSION\_NUMBER, OpenSSL\_version,
OpenSSL\_version\_num - get OpenSSL version number

# SYNOPSIS

    #include <openssl/opensslv.h>
    #define OPENSSL_VERSION_NUMBER 0xnnnnnnnnnL

    #include <openssl/crypto.h>

    unsigned long OpenSSL_version_num();
    const char *OpenSSL_version(int t);

# DESCRIPTION

OPENSSL\_VERSION\_NUMBER is a numeric release version identifier:

    MNNFFPPS: major minor fix patch status

The status nibble has one of the values 0 for development, 1 to e for betas
1 to 14, and f for release.

for example

    0x000906000 == 0.9.6 dev
    0x000906023 == 0.9.6b beta 3
    0x00090605f == 0.9.6e release

Versions prior to 0.9.3 have identifiers < 0x0930.
Versions between 0.9.3 and 0.9.5 had a version identifier with this
interpretation:

    MMNNFFRBB major minor fix final beta/patch

for example

    0x000904100 == 0.9.4 release
    0x000905000 == 0.9.5 dev

Version 0.9.5a had an interim interpretation that is like the current one,
except the patch level got the highest bit set, to keep continuity.  The
number was therefore 0x0090581f.

OpenSSL\_version\_num() returns the version number.

OpenSSL\_version() returns different strings depending on **t**:

- OPENSSL\_VERSION

    The text variant of the version number and the release date.  For example,
    "OpenSSL 1.0.1a 15 Oct 2015".

- OPENSSL\_CFLAGS

    The compiler flags set for the compilation process in the form
    "compiler: ..."  if available or "compiler: information not available"
    otherwise.

- OPENSSL\_BUILT\_ON

    The date of the build process in the form "built on: ..." if available
    or "built on: date not available" otherwise.

- OPENSSL\_PLATFORM

    The "Configure" target of the library build in the form "platform: ..."
    if available or "platform: information not available" otherwise.

- OPENSSL\_DIR

    The "OPENSSLDIR" setting of the library build in the form "OPENSSLDIR: "...""
    if available or "OPENSSLDIR: N/A" otherwise.

- OPENSSL\_ENGINES\_DIR

    The "ENGINESDIR" setting of the library build in the form "ENGINESDIR: "...""
    if available or "ENGINESDIR: N/A" otherwise.

For an unknown **t**, the text "not available" is returned.

# RETURN VALUE

The version number.

# SEE ALSO

[crypto(3)](http://man.he.net/man3/crypto)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
