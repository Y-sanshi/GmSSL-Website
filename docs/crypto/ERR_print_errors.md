# NAME

ERR\_print\_errors, ERR\_print\_errors\_fp, ERR\_print\_errors\_cb
\- print error messages

# SYNOPSIS

    #include <openssl/err.h>

    void ERR_print_errors(BIO *bp);
    void ERR_print_errors_fp(FILE *fp);
    void ERR_print_errors_cb(int (*cb)(const char *str, size_t len, void *u),
                             void *u)

# DESCRIPTION

ERR\_print\_errors() is a convenience function that prints the error
strings for all errors that OpenSSL has recorded to **bp**, thus
emptying the error queue.

ERR\_print\_errors\_fp() is the same, except that the output goes to a
**FILE**.

ERR\_print\_errors\_cb() is the same, except that the callback function,
**cb**, is called for each error line with the string, length, and userdata
**u** as the callback parameters.

The error strings will have the following format:

    [pid]:error:[error code]:[library name]:[function name]:[reason string]:[file name]:[line]:[optional text message]

_error code_ is an 8 digit hexadecimal number. _library name_,
_function name_ and _reason string_ are ASCII text, as is _optional
text message_ if one was set for the respective error code.

If there is no text string registered for the given error code,
the error string will contain the numeric code.

# RETURN VALUES

ERR\_print\_errors() and ERR\_print\_errors\_fp() return no values.

# SEE ALSO

[err(3)](http://man.he.net/man3/err), [ERR\_error\_string(3)](http://man.he.net/man3/ERR_error_string),
[ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error).

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
