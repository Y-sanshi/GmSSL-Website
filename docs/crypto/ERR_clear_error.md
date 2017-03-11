# NAME

ERR\_clear\_error - clear the error queue

# SYNOPSIS

    #include <openssl/err.h>

    void ERR_clear_error(void);

# DESCRIPTION

ERR\_clear\_error() empties the current thread's error queue.

# RETURN VALUES

ERR\_clear\_error() has no return value.

# SEE ALSO

[err(3)](http://man.he.net/man3/err), [ERR\_get\_error(3)](http://man.he.net/man3/ERR_get_error)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
