# NAME

ERR\_error\_string, ERR\_error\_string\_n, ERR\_lib\_error\_string,
ERR\_func\_error\_string, ERR\_reason\_error\_string - obtain human-readable
error message

# SYNOPSIS

    #include <openssl/err.h>

    char *ERR_error_string(unsigned long e, char *buf);
    void ERR_error_string_n(unsigned long e, char *buf, size_t len);

    const char *ERR_lib_error_string(unsigned long e);
    const char *ERR_func_error_string(unsigned long e);
    const char *ERR_reason_error_string(unsigned long e);

# DESCRIPTION

ERR\_error\_string() generates a human-readable string representing the
error code _e_, and places it at _buf_. _buf_ must be at least 256
bytes long. If _buf_ is **NULL**, the error string is placed in a
static buffer.
Note that this function is not thread-safe and does no checks on the size
of the buffer; use ERR\_error\_string\_n() instead.

ERR\_error\_string\_n() is a variant of ERR\_error\_string() that writes
at most _len_ characters (including the terminating 0)
and truncates the string if necessary.
For ERR\_error\_string\_n(), _buf_ may not be **NULL**.

The string will have the following format:

    error:[error code]:[library name]:[function name]:[reason string]

_error code_ is an 8 digit hexadecimal number, _library name_,
_function name_ and _reason string_ are ASCII text.

ERR\_lib\_error\_string(), ERR\_func\_error\_string() and
ERR\_reason\_error\_string() return the library name, function
name and reason string respectively.

If there is no text string registered for the given error code,
the error string will contain the numeric code.

[ERR\_print\_errors(3)](http://man.he.net/man3/ERR_print_errors) can be used to print
all error codes currently in the queue.

# RETURN VALUES

ERR\_error\_string() returns a pointer to a static buffer containing the
string if _buf_ **== NULL**, _buf_ otherwise.

ERR\_lib\_error\_string(), ERR\_func\_error\_string() and
ERR\_reason\_error\_string() return the strings, and **NULL** if
none is registered for the error code.

# SEE ALSO

[err(3)](http://man.he.net/man3/err), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error),
[ERR\_print\_errors(3)](http://man.he.net/man3/ERR_print_errors)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
