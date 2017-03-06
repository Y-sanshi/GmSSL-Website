# NAME

version - print GmSSL version information

# SYNOPSIS

**gmssl version**
\[**-help**\]
\[**-a**\]
\[**-v**\]
\[**-b**\]
\[**-o**\]
\[**-f**\]
\[**-p**\]
\[**-d**\]
\[**-e**\]

# DESCRIPTION

This command is used to print out version information about GmSSL.

# OPTIONS

- **-help**

    Print out a usage message.

- **-a**

    all information, this is the same as setting all the other flags.

- **-v**

    the current GmSSL version.

- **-b**

    the date the current version of GmSSL was built.

- **-o**

    option information: various options set when the library was built.

- **-f**

    compilation flags.

- **-p**

    platform setting.

- **-d**

    OPENSSLDIR setting.

- **-e**

    ENGINESDIR setting.

# NOTES

The output of **gmssl version -a** would typically be used when sending
in a bug report.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the GmSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
