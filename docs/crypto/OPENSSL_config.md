# NAME

OPENSSL\_config, OPENSSL\_no\_config - simple OpenSSL configuration functions

# SYNOPSIS

    #include <openssl/conf.h>

    #if OPENSSL_API_COMPAT < 0x10100000L
    void OPENSSL_config(const char *appname);
    void OPENSSL_no_config(void);
    #endif

# DESCRIPTION

OPENSSL\_config() configures OpenSSL using the standard **openssl.cnf** and
reads from the application section **appname**. If **appname** is NULL then
the default section, **openssl\_conf**, will be used.
Errors are silently ignored.
Multiple calls have no effect.

OPENSSL\_no\_config() disables configuration. If called before OPENSSL\_config()
no configuration takes place.

If the application is built with **OPENSSL\_LOAD\_CONF** defined, then a
call to OpenSSL\_add\_all\_algorithms() will implicitly call OPENSSL\_config()
first.

# NOTES

The OPENSSL\_config() function is designed to be a very simple "call it and
forget it" function.
It is however **much** better than nothing. Applications which need finer
control over their configuration functionality should use the configuration
functions such as CONF\_modules\_load() directly. This function is deprecated
and its use should be avoided.
Applications should instead call CONF\_modules\_load() during
initialization (that is before starting any threads).

There are several reasons why calling the OpenSSL configuration routines is
advisable. For example, to load dynamic ENGINEs from shared libraries (DSOs).
However very few applications currently support the control interface and so
very few can load and use dynamic ENGINEs. Equally in future more sophisticated
ENGINEs will require certain control operations to customize them. If an
application calls OPENSSL\_config() it doesn't need to know or care about
ENGINE control operations because they can be performed by editing a
configuration file.

# RETURN VALUES

Neither OPENSSL\_config() nor OPENSSL\_no\_config() return a value.

# SEE ALSO

[conf(5)](http://man.he.net/man5/conf),
[CONF\_modules\_load\_file(3)](http://man.he.net/man3/CONF_modules_load_file)

# HISTORY

The OPENSSL\_no\_config() and OPENSSL\_config() functions were
deprecated in OpenSSL 1.1.0 by OPENSSL\_init\_crypto().

# COPYRIGHT

Copyright 2004-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
