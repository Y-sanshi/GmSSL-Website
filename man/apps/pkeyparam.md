# NAME

pkeyparam - public key algorithm parameter processing tool

# SYNOPSIS

**gmssl** **pkeyparam**
\[**-help**\]
\[**-in filename**\]
\[**-out filename**\]
\[**-text**\]
\[**-noout**\]
\[**-engine id**\]

# DESCRIPTION

The **pkey** command processes public or private keys. They can be converted
between various forms and their components printed out.

# OPTIONS

- **-help**

    Print out a usage message.

- **-in filename**

    This specifies the input filename to read parameters from or standard input if
    this option is not specified.

- **-out filename**

    This specifies the output filename to write parameters to or standard output if
    this option is not specified.

- **-text**

    prints out the parameters in plain text in addition to the encoded version.

- **-noout**

    do not output the encoded version of the parameters.

- **-engine id**

    specifying an engine (by its unique **id** string) will cause **pkeyparam**
    to attempt to obtain a functional reference to the specified engine,
    thus initialising it if needed. The engine will then be set as the default
    for all available algorithms.

# EXAMPLE

Print out text version of parameters:

    gmssl pkeyparam -in param.pem -text

# NOTES

There are no **-inform** or **-outform** options for this command because only
PEM format is supported because the key type is determined by the PEM headers.

# SEE ALSO

[genpkey(1)](http://man.he.net/man1/genpkey), [rsa(1)](http://man.he.net/man1/rsa), [pkcs8(1)](http://man.he.net/man1/pkcs8),
[dsa(1)](http://man.he.net/man1/dsa), [genrsa(1)](http://man.he.net/man1/genrsa), [gendsa(1)](http://man.he.net/man1/gendsa)

# COPYRIGHT

Copyright 2006-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the GmSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
