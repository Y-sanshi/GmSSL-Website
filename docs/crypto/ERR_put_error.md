# NAME

ERR\_put\_error, ERR\_add\_error\_data - record an error

# SYNOPSIS

    #include <openssl/err.h>

    void ERR_put_error(int lib, int func, int reason, const char *file,
            int line);

    void ERR_add_error_data(int num, ...);
    void ERR_add_error_data(int num, va_list arg);

# DESCRIPTION

ERR\_put\_error() adds an error code to the thread's error queue. It
signals that the error of reason code **reason** occurred in function
**func** of library **lib**, in line number **line** of **file**.
This function is usually called by a macro.

ERR\_add\_error\_data() associates the concatenation of its **num** string
arguments with the error code added last.
ERR\_add\_error\_vdata() is similar except the argument is a **va\_list**.

[ERR\_load\_strings(3)](http://man.he.net/man3/ERR_load_strings) can be used to register
error strings so that the application can a generate human-readable
error messages for the error code.

## Reporting errors

Each sub-library has a specific macro XXXerr() that is used to report
errors. Its first argument is a function code **XXX\_F\_...**, the second
argument is a reason code **XXX\_R\_...**. Function codes are derived
from the function names; reason codes consist of textual error
descriptions. For example, the function ssl3\_read\_bytes() reports a
"handshake failure" as follows:

    SSLerr(SSL_F_SSL3_READ_BYTES, SSL_R_SSL_HANDSHAKE_FAILURE);

Function and reason codes should consist of upper case characters,
numbers and underscores only. The error file generation script translates
function codes into function names by looking in the header files
for an appropriate function name, if none is found it just uses
the capitalized form such as "SSL3\_READ\_BYTES" in the above example.

The trailing section of a reason code (after the "\_R\_") is translated
into lower case and underscores changed to spaces.

Although a library will normally report errors using its own specific
XXXerr macro, another library's macro can be used. This is normally
only done when a library wants to include ASN1 code which must use
the ASN1err() macro.

# RETURN VALUES

ERR\_put\_error() and ERR\_add\_error\_data() return
no values.

# SEE ALSO

[err(3)](http://man.he.net/man3/err), [ERR\_load\_strings(3)](http://man.he.net/man3/ERR_load_strings)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
