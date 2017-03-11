# NAME

ERR\_remove\_thread\_state, ERR\_remove\_state - DEPRECATED

# SYNOPSIS

Deprecated:

    #if OPENSSL_API_COMPAT < 0x10000000L
    void ERR_remove_state(unsigned long pid);
    #endif

    #if OPENSSL_API_COMPAT < 0x10100000L
    void ERR_remove_thread_state(void *);
    #endif

# DESCRIPTION

The functions described here were used to free the error queue
associated with the current or specified thread.

They are now deprecated and do nothing, as the OpenSSL libraries now
normally do all thread initialisation and deinitialisation
automatically (see [OPENSSL\_init\_crypto(3)](http://man.he.net/man3/OPENSSL_init_crypto)).

# RETURN VALUE

The functions described here return no value.

# SEE ALSO

[err(3)](http://man.he.net/man3/err), [OPENSSL\_init\_crypto(3)](http://man.he.net/man3/OPENSSL_init_crypto)

# HISTORY

ERR\_remove\_state() was deprecated in OpenSSL 1.0.0 when
ERR\_remove\_thread\_state() was introduced.

ERR\_remove\_thread\_state() was deprecated in OpenSSL 1.1.0 when the
thread handling functionality was entirely rewritten.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
