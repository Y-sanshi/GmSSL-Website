# NAME

nseq - create or examine a Netscape certificate sequence

# SYNOPSIS

**gmssl** **nseq**
\[**-help**\]
\[**-in filename**\]
\[**-out filename**\]
\[**-toseq**\]

# DESCRIPTION

The **nseq** command takes a file containing a Netscape certificate
sequence and prints out the certificates contained in it or takes a
file of certificates and converts it into a Netscape certificate
sequence.

# OPTIONS

- **-help**

    Print out a usage message.

- **-in filename**

    This specifies the input filename to read or standard input if this
    option is not specified.

- **-out filename**

    specifies the output filename or standard output by default.

- **-toseq**

    normally a Netscape certificate sequence will be input and the output
    is the certificates contained in it. With the **-toseq** option the
    situation is reversed: a Netscape certificate sequence is created from
    a file of certificates.

# EXAMPLES

Output the certificates in a Netscape certificate sequence

    gmssl nseq -in nseq.pem -out certs.pem

Create a Netscape certificate sequence

    gmssl nseq -in certs.pem -toseq -out nseq.pem

# NOTES

The **PEM** encoded form uses the same headers and footers as a certificate:

    -----BEGIN CERTIFICATE-----
    -----END CERTIFICATE-----

A Netscape certificate sequence is a Netscape specific form that can be sent
to browsers as an alternative to the standard PKCS#7 format when several
certificates are sent to the browser: for example during certificate enrollment.
It is used by Netscape certificate server for example.

# BUGS

This program needs a few more options: like allowing DER or PEM input and
output files and allowing multiple certificate files to be used.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the GmSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).