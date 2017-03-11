# NAME

ERR\_load\_strings, ERR\_PACK, ERR\_get\_next\_error\_library - load
arbitrary error strings

# SYNOPSIS

    #include <openssl/err.h>

    void ERR_load_strings(int lib, ERR_STRING_DATA str[]);

    int ERR_get_next_error_library(void);

    unsigned long ERR_PACK(int lib, int func, int reason);

# DESCRIPTION

ERR\_load\_strings() registers error strings for library number **lib**.

**str** is an array of error string data:

    typedef struct ERR_string_data_st
    {
           unsigned long error;
           char *string;
    } ERR_STRING_DATA;

The error code is generated from the library number and a function and
reason code: **error** = ERR\_PACK(**lib**, **func**, **reason**).
ERR\_PACK() is a macro.

The last entry in the array is {0,0}.

ERR\_get\_next\_error\_library() can be used to assign library numbers
to user libraries at runtime.

# RETURN VALUE

ERR\_load\_strings() returns no value. ERR\_PACK() return the error code.
ERR\_get\_next\_error\_library() returns zero on failure, otherwise a new
library number.

# SEE ALSO

[err(3)](http://man.he.net/man3/err), [ERR\_load\_strings(3)](http://man.he.net/man3/ERR_load_strings)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
