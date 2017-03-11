# NAME

ERR\_GET\_LIB, ERR\_GET\_FUNC, ERR\_GET\_REASON, ERR\_FATAL\_ERROR
\- get information from error codes

# SYNOPSIS

    #include <openssl/err.h>

    int ERR_GET_LIB(unsigned long e);

    int ERR_GET_FUNC(unsigned long e);

    int ERR_GET_REASON(unsigned long e);

    int ERR_FATAL_ERROR(unsigned long e);

# DESCRIPTION

The error code returned by ERR\_get\_error() consists of a library
number, function code and reason code. ERR\_GET\_LIB(), ERR\_GET\_FUNC()
and ERR\_GET\_REASON() can be used to extract these.

ERR\_FATAL\_ERROR() indicates whether a given error code is a fatal error.

The library number and function code describe where the error
occurred, the reason code is the information about what went wrong.

Each sub-library of OpenSSL has a unique library number; function and
reason codes are unique within each sub-library.  Note that different
libraries may use the same value to signal different functions and
reasons.

**ERR\_R\_...** reason codes such as **ERR\_R\_MALLOC\_FAILURE** are globally
unique. However, when checking for sub-library specific reason codes,
be sure to also compare the library number.

ERR\_GET\_LIB(), ERR\_GET\_FUNC(), ERR\_GET\_REASON(), and ERR\_FATAL\_ERROR()
 are macros.

# RETURN VALUES

The library number, function code, reason code, and whether the error
is fatal, respectively.

# SEE ALSO

[err(7)](http://man.he.net/man7/err), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error)

# HISTORY

ERR\_GET\_LIB(), ERR\_GET\_FUNC() and ERR\_GET\_REASON() are available in
all versions of OpenSSL.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
