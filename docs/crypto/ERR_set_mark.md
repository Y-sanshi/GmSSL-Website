# NAME

ERR\_set\_mark, ERR\_pop\_to\_mark - set marks and pop errors until mark

# SYNOPSIS

    #include <openssl/err.h>

    int ERR_set_mark(void);

    int ERR_pop_to_mark(void);

# DESCRIPTION

ERR\_set\_mark() sets a mark on the current topmost error record if there
is one.

ERR\_pop\_to\_mark() will pop the top of the error stack until a mark is found.
The mark is then removed.  If there is no mark, the whole stack is removed.

# RETURN VALUES

ERR\_set\_mark() returns 0 if the error stack is empty, otherwise 1.

ERR\_pop\_to\_mark() returns 0 if there was no mark in the error stack, which
implies that the stack became empty, otherwise 1.

# SEE ALSO

[err(3)](http://man.he.net/man3/err)

# COPYRIGHT

Copyright 2003-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
