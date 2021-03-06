# NAME

errstr - lookup error codes

# SYNOPSIS

**gmssl errstr error\_code**

# DESCRIPTION

Sometimes an application will not load error message and only
numerical forms will be available. The **errstr** utility can be used to
display the meaning of the hex code. The hex code is the hex digits after the
second colon.

# OPTIONS

None.

# EXAMPLE

The error code:

    27594:error:2006D080:lib(32):func(109):reason(128):bss_file.c:107:

can be displayed with:

    gmssl errstr 2006D080

to produce the error message:

    error:2006D080:BIO routines:BIO_new_file:no such file

# COPYRIGHT

Copyright 2004-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the GmSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
