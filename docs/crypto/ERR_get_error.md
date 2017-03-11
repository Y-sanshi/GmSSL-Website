# NAME

ERR\_get\_error, ERR\_peek\_error, ERR\_peek\_last\_error,
ERR\_get\_error\_line, ERR\_peek\_error\_line, ERR\_peek\_last\_error\_line,
ERR\_get\_error\_line\_data, ERR\_peek\_error\_line\_data,
ERR\_peek\_last\_error\_line\_data - obtain error code and data

# SYNOPSIS

    #include <openssl/err.h>

    unsigned long ERR_get_error(void);
    unsigned long ERR_peek_error(void);
    unsigned long ERR_peek_last_error(void);

    unsigned long ERR_get_error_line(const char **file, int *line);
    unsigned long ERR_peek_error_line(const char **file, int *line);
    unsigned long ERR_peek_last_error_line(const char **file, int *line);

    unsigned long ERR_get_error_line_data(const char **file, int *line,
            const char **data, int *flags);
    unsigned long ERR_peek_error_line_data(const char **file, int *line,
            const char **data, int *flags);
    unsigned long ERR_peek_last_error_line_data(const char **file, int *line,
            const char **data, int *flags);

# DESCRIPTION

ERR\_get\_error() returns the earliest error code from the thread's error
queue and removes the entry. This function can be called repeatedly
until there are no more error codes to return.

ERR\_peek\_error() returns the earliest error code from the thread's
error queue without modifying it.

ERR\_peek\_last\_error() returns the latest error code from the thread's
error queue without modifying it.

See [ERR\_GET\_LIB(3)](http://man.he.net/man3/ERR_GET_LIB) for obtaining information about
location and reason of the error, and
[ERR\_error\_string(3)](http://man.he.net/man3/ERR_error_string) for human-readable error
messages.

ERR\_get\_error\_line(), ERR\_peek\_error\_line() and
ERR\_peek\_last\_error\_line() are the same as the above, but they
additionally store the file name and line number where
the error occurred in \***file** and \***line**, unless these are **NULL**.

ERR\_get\_error\_line\_data(), ERR\_peek\_error\_line\_data() and
ERR\_peek\_last\_error\_line\_data() store additional data and flags
associated with the error code in \***data**
and \***flags**, unless these are **NULL**. \***data** contains a string
if \***flags**&**ERR\_TXT\_STRING** is true.

An application **MUST NOT** free the \***data** pointer (or any other pointers
returned by these functions) with OPENSSL\_free() as freeing is handled
automatically by the error library.

# RETURN VALUES

The error code, or 0 if there is no error in the queue.

# SEE ALSO

[err(3)](http://man.he.net/man3/err), [ERR\_error\_string(3)](http://man.he.net/man3/ERR_error_string),
[ERR\_GET\_LIB(3)](http://man.he.net/man3/ERR_GET_LIB)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
