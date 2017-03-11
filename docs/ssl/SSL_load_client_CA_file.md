# NAME

SSL\_load\_client\_CA\_file - load certificate names from file

# SYNOPSIS

    #include <openssl/ssl.h>

    STACK_OF(X509_NAME) *SSL_load_client_CA_file(const char *file);

# DESCRIPTION

SSL\_load\_client\_CA\_file() reads certificates from **file** and returns
a STACK\_OF(X509\_NAME) with the subject names found.

# NOTES

SSL\_load\_client\_CA\_file() reads a file of PEM formatted certificates and
extracts the X509\_NAMES of the certificates found. While the name suggests
the specific usage as support function for
[SSL\_CTX\_set\_client\_CA\_list(3)](http://man.he.net/man3/SSL_CTX_set_client_CA_list),
it is not limited to CA certificates.

# EXAMPLES

Load names of CAs from file and use it as a client CA list:

    SSL_CTX *ctx;
    STACK_OF(X509_NAME) *cert_names;

    ...
    cert_names = SSL_load_client_CA_file("/path/to/CAfile.pem");
    if (cert_names != NULL)
      SSL_CTX_set_client_CA_list(ctx, cert_names);
    else
      error_handling();
    ...

# RETURN VALUES

The following return values can occur:

- NULL

    The operation failed, check out the error stack for the reason.

- Pointer to STACK\_OF(X509\_NAME)

    Pointer to the subject names of the successfully read certificates.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_set\_client\_CA\_list(3)](http://man.he.net/man3/SSL_CTX_set_client_CA_list)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
