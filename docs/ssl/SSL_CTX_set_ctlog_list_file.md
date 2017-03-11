# NAME

SSL\_CTX\_set\_default\_ctlog\_list\_file, SSL\_CTX\_set\_ctlog\_list\_file -
load a Certificate Transparency log list from a file

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CTX_set_default_ctlog_list_file(SSL_CTX *ctx);
    int SSL_CTX_set_ctlog_list_file(SSL_CTX *ctx, const char *path);

# DESCRIPTION

SSL\_CTX\_set\_default\_ctlog\_list\_file() loads a list of Certificate Transparency
(CT) logs from the default file location, "ct\_log\_list.cnf", found in the
directory where OpenSSL is installed.

SSL\_CTX\_set\_ctlog\_list\_file() loads a list of CT logs from a specific path.
See [CTLOG\_STORE\_new(3)](http://man.he.net/man3/CTLOG_STORE_new) for the file format.

# NOTES

These functions will not clear the existing CT log list - it will be appended
to. To replace the existing list, use [SSL\_CTX\_set0\_ctlog\_store](https://metacpan.org/pod/SSL_CTX_set0_ctlog_store) first. 

If an error occurs whilst parsing a particular log entry in the file, that log
entry will be skipped.

# RETURN VALUES

SSL\_CTX\_set\_default\_ctlog\_list\_file() and SSL\_CTX\_set\_ctlog\_list\_file()
return 1 if the log list is successfully loaded, and 0 if an error occurs. In
the case of an error, the log list may have been partially loaded.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_set\_ct\_validation\_callback(3)](http://man.he.net/man3/SSL_CTX_set_ct_validation_callback),
[CTLOG\_STORE\_new(3)](http://man.he.net/man3/CTLOG_STORE_new)

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
