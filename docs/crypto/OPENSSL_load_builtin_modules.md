# NAME

OPENSSL\_load\_builtin\_modules, ASN1\_add\_oid\_module, ENGINE\_add\_conf\_module - add standard configuration modules

# SYNOPSIS

    #include <openssl/conf.h>

    void OPENSSL_load_builtin_modules(void);
    void ASN1_add_oid_module(void);
    ENGINE_add_conf_module();

# DESCRIPTION

The function OPENSSL\_load\_builtin\_modules() adds all the standard OpenSSL
configuration modules to the internal list. They can then be used by the
OpenSSL configuration code.

ASN1\_add\_oid\_module() adds just the ASN1 OBJECT module.

ENGINE\_add\_conf\_module() adds just the ENGINE configuration module.

# NOTES

If the simple configuration function OPENSSL\_config() is called then
OPENSSL\_load\_builtin\_modules() is called automatically.

Applications which use the configuration functions directly will need to
call OPENSSL\_load\_builtin\_modules() themselves _before_ any other
configuration code.

Applications should call OPENSSL\_load\_builtin\_modules() to load all
configuration modules instead of adding modules selectively: otherwise
functionality may be missing from the application if an when new
modules are added.

# RETURN VALUE

None of the functions return a value.

# SEE ALSO

[conf(3)](http://man.he.net/man3/conf), [OPENSSL\_config(3)](http://man.he.net/man3/OPENSSL_config)

# COPYRIGHT

Copyright 2004-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
