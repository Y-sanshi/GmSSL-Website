# NAME

CONF\_modules\_free, CONF\_modules\_finish, CONF\_modules\_unload -
OpenSSL configuration cleanup functions

# SYNOPSIS

    #include <openssl/conf.h>

    void CONF_modules_finish(void);
    void CONF_modules_unload(int all);

Deprecated:

    #if OPENSSL_API_COMPAT < 0x10100000L
    void CONF_modules_free(void)
    #endif

# DESCRIPTION

CONF\_modules\_free() closes down and frees up all memory allocated by all
configuration modules.

CONF\_modules\_finish() calls each configuration modules **finish** handler
to free up any configuration that module may have performed.

CONF\_modules\_unload() finishes and unloads configuration modules. If
**all** is set to **0** only modules loaded from DSOs will be unloads. If
**all** is **1** all modules, including builtin modules will be unloaded.

# NOTES

Normally in versions of OpenSSL prior to 1.1.0 applications will only call
CONF\_modules\_free() at application exit to tidy up any configuration performed.
From 1.1.0 CONF\_modules\_free() is deprecated and no explicit CONF cleanup is
required at all. For more information see [OPENSSL\_init\_crypto(3)](http://man.he.net/man3/OPENSSL_init_crypto).

# RETURN VALUE

None of the functions return a value.

# SEE ALSO

[conf(5)](http://man.he.net/man5/conf), [OPENSSL\_config(3)](http://man.he.net/man3/OPENSSL_config),
[CONF\_modules\_load\_file(3)](http://man.he.net/man3/CONF_modules_load_file)

# HISTORY

CONF\_modules\_free() was deprecated in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2004-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
