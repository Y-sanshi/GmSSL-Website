# NAME

SSL\_CTX\_config, SSL\_config - configure SSL\_CTX or SSL structure

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CTX_config(SSL_CTX *ctx, const char *name);
    int SSL_config(SSL *s, const char *name);

# DESCRIPTION

The functions SSL\_CTX\_config() and SSL\_config() configure an **SSL\_CTX** or
**SSL** structure using the configuration **name**.

# NOTES

By calling SSL\_CTX\_config() or SSL\_config() an application can perform many
complex tasks based on the contents of the configuration file: greatly
simplifying application configuration code. A degree of future proofing
can also be achieved: an application can support configuration features
in newer versions of OpenSSL automatically.

A configuration file must have been previously loaded, for example using
CONF\_modules\_load\_file(). See [config(3)](http://man.he.net/man3/config) for details of the configuration
file syntax.

# RETURN VALUES

SSL\_CTX\_config() and SSL\_config() return 1 for success or 0 if an error
occurred.

# EXAMPLE

If the file "config.cnf" contains the following:

    testapp = test_sect

    [test_sect]
    # list of confuration modules

    ssl_conf = ssl_sect

    [ssl_sect]

    server = server_section

    [server_section]

    RSA.Certificate = server-rsa.pem
    ECDSA.Certificate = server-ecdsa.pem
    Ciphers = ALL:!RC4

An application could call:

    if (CONF_modules_load_file("config.cnf", "testapp", 0) <= 0) {
         fprintf(stderr, "Error processing config file\n");
         goto err;
    }

    ctx = SSL_CTX_new(TLS_server_method());

    if (SSL_CTX_config(ctx, "server") == 0) {
        fprintf(stderr, "Error configuring server.\n");
        goto err;
    }

In this example two certificates and the cipher list are configured without
the need for any additional application code.

# SEE ALSO

[config(3)](http://man.he.net/man3/config),
[SSL\_CONF\_cmd(3)](http://man.he.net/man3/SSL_CONF_cmd),
[CONF\_modules\_load\_file(3)](http://man.he.net/man3/CONF_modules_load_file)

# HISTORY

SSL\_CTX\_config() and SSL\_config() were first added to OpenSSL 1.1.0

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
