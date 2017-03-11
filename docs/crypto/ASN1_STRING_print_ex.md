# NAME

ASN1\_STRING\_print\_ex, ASN1\_STRING\_print\_ex\_fp, ASN1\_STRING\_print - ASN1\_STRING output routines

# SYNOPSIS

    #include <openssl/asn1.h>

    int ASN1_STRING_print_ex(BIO *out, const ASN1_STRING *str, unsigned long flags);
    int ASN1_STRING_print_ex_fp(FILE *fp, const ASN1_STRING *str, unsigned long flags);
    int ASN1_STRING_print(BIO *out, const ASN1_STRING *str);

# DESCRIPTION

These functions output an **ASN1\_STRING** structure. **ASN1\_STRING** is used to
represent all the ASN1 string types.

ASN1\_STRING\_print\_ex() outputs **str** to **out**, the format is determined by
the options **flags**. ASN1\_STRING\_print\_ex\_fp() is identical except it outputs
to **fp** instead.

ASN1\_STRING\_print() prints **str** to **out** but using a different format to
ASN1\_STRING\_print\_ex(). It replaces unprintable characters (other than CR, LF)
with '.'.

# NOTES

ASN1\_STRING\_print() is a legacy function which should be avoided in new applications.

Although there are a large number of options frequently **ASN1\_STRFLGS\_RFC2253** is
suitable, or on UTF8 terminals **ASN1\_STRFLGS\_RFC2253 & ~ASN1\_STRFLGS\_ESC\_MSB**.

The complete set of supported options for **flags** is listed below.

Various characters can be escaped. If **ASN1\_STRFLGS\_ESC\_2253** is set the characters
determined by RFC2253 are escaped. If **ASN1\_STRFLGS\_ESC\_CTRL** is set control
characters are escaped. If **ASN1\_STRFLGS\_ESC\_MSB** is set characters with the
MSB set are escaped: this option should **not** be used if the terminal correctly
interprets UTF8 sequences.

Escaping takes several forms.

If the character being escaped is a 16 bit character then the form "\\UXXXX" is used
using exactly four characters for the hex representation. If it is 32 bits then
"\\WXXXXXXXX" is used using eight characters of its hex representation. These forms
will only be used if UTF8 conversion is not set (see below).

Printable characters are normally escaped using the backslash '\\' character. If
**ASN1\_STRFLGS\_ESC\_QUOTE** is set then the whole string is instead surrounded by
double quote characters: this is arguably more readable than the backslash
notation. Other characters use the "\\XX" using exactly two characters of the hex
representation.

If **ASN1\_STRFLGS\_UTF8\_CONVERT** is set then characters are converted to UTF8
format first. If the terminal supports the display of UTF8 sequences then this
option will correctly display multi byte characters.

If **ASN1\_STRFLGS\_IGNORE\_TYPE** is set then the string type is not interpreted at
all: everything is assumed to be one byte per character. This is primarily for
debugging purposes and can result in confusing output in multi character strings.

If **ASN1\_STRFLGS\_SHOW\_TYPE** is set then the string type itself is printed out
before its value (for example "BMPSTRING"), this actually uses ASN1\_tag2str().

The content of a string instead of being interpreted can be "dumped": this just
outputs the value of the string using the form #XXXX using hex format for each
octet.

If **ASN1\_STRFLGS\_DUMP\_ALL** is set then any type is dumped.

Normally non character string types (such as OCTET STRING) are assumed to be
one byte per character, if **ASN1\_STRFLGS\_DUMP\_UNKNOWN** is set then they will
be dumped instead.

When a type is dumped normally just the content octets are printed, if
**ASN1\_STRFLGS\_DUMP\_DER** is set then the complete encoding is dumped
instead (including tag and length octets).

**ASN1\_STRFLGS\_RFC2253** includes all the flags required by RFC2253. It is
equivalent to:
 ASN1\_STRFLGS\_ESC\_2253 | ASN1\_STRFLGS\_ESC\_CTRL | ASN1\_STRFLGS\_ESC\_MSB |
 ASN1\_STRFLGS\_UTF8\_CONVERT | ASN1\_STRFLGS\_DUMP\_UNKNOWN ASN1\_STRFLGS\_DUMP\_DER

# SEE ALSO

[X509\_NAME\_print\_ex(3)](http://man.he.net/man3/X509_NAME_print_ex),
[ASN1\_tag2str(3)](http://man.he.net/man3/ASN1_tag2str)

# COPYRIGHT

Copyright 2002-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
