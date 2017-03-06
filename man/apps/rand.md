# NAME

rand - generate pseudo-random bytes

# SYNOPSIS

**gmssl rand**
\[**-help**\]
\[**-out** _file_\]
\[**-rand** _file(s)_\]
\[**-base64**\]
\[**-hex**\]
_num_

# DESCRIPTION

The **rand** command outputs _num_ pseudo-random bytes after seeding
the random number generator once.  As in other **gmssl** command
line tools, PRNG seeding uses the file _$HOME/_**.rnd** or **.rnd**
in addition to the files given in the **-rand** option.  A new
_$HOME_/**.rnd** or **.rnd** file will be written back if enough
seeding was obtained from these sources.

# OPTIONS

- **-help**

    Print out a usage message.

- **-out** _file_

    Write to _file_ instead of standard output.

- **-rand** _file(s)_

    Use specified file or files or EGD socket (see [RAND\_egd(3)](http://man.he.net/man3/RAND_egd))
    for seeding the random number generator.
    Multiple files can be specified separated by an OS-dependent character.
    The separator is **;** for MS-Windows, **,** for OpenVMS, and **:** for
    all others.

- **-base64**

    Perform base64 encoding on the output.

- **-hex**

    Show the output as a hex string.

# SEE ALSO

[RAND\_bytes(3)](http://man.he.net/man3/RAND_bytes)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the GmSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
