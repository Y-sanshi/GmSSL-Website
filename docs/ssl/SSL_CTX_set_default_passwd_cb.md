# NAME

SSL\_CTX\_set\_default\_passwd\_cb, SSL\_CTX\_set\_default\_passwd\_cb\_userdata,
SSL\_CTX\_get\_default\_passwd\_cb, SSL\_CTX\_get\_default\_passwd\_cb\_userdata,
SSL\_set\_default\_passwd\_cb, SSL\_set\_default\_passwd\_cb\_userdata,
SSL\_get\_default\_passwd\_cb, SSL\_get\_default\_passwd\_cb\_userdata - set or
get passwd callback for encrypted PEM file handling

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_CTX_set_default_passwd_cb(SSL_CTX *ctx, pem_password_cb *cb);
    void SSL_CTX_set_default_passwd_cb_userdata(SSL_CTX *ctx, void *u);
    pem_password_cb *SSL_CTX_get_default_passwd_cb(SSL_CTX *ctx);
    void *SSL_CTX_get_default_passwd_cb_userdata(SSL_CTX *ctx);

    void SSL_set_default_passwd_cb(SSL *s, pem_password_cb *cb);
    void SSL_set_default_passwd_cb_userdata(SSL *s, void *u);
    pem_password_cb *SSL_get_default_passwd_cb(SSL *s);
    void *SSL_get_default_passwd_cb_userdata(SSL *s);

# DESCRIPTION

SSL\_CTX\_set\_default\_passwd\_cb() sets the default password callback called
when loading/storing a PEM certificate with encryption.

SSL\_CTX\_set\_default\_passwd\_cb\_userdata() sets a pointer to userdata, **u**,
which will be provided to the password callback on invocation.

SSL\_CTX\_get\_default\_passwd\_cb() returns a function pointer to the password
callback currently set in **ctx**. If no callback was explicitly set, the
NULL pointer is returned.

SSL\_CTX\_get\_default\_passwd\_cb\_userdata() returns a pointer to the userdata
currently set in **ctx**. If no userdata was explicitly set, the NULL pointer
is returned.

SSL\_set\_default\_passwd\_cb(), SSL\_set\_default\_passwd\_cb\_userdata(),
SSL\_get\_default\_passwd\_cb() and SSL\_get\_default\_passwd\_cb\_userdata() perform
the same function as their SSL\_CTX counterparts, but using an SSL object.

The password callback, which must be provided by the application, hands back the
password to be used during decryption.
On invocation a pointer to userdata
is provided. The function must store the password into the provided buffer
**buf** which is of size **size**. The actual length of the password must
be returned to the calling function. **rwflag** indicates whether the
callback is used for reading/decryption (rwflag=0) or writing/encryption
(rwflag=1).
For more details, see [pem\_password\_cb(3)](http://man.he.net/man3/pem_password_cb).

# NOTES

When loading or storing private keys, a password might be supplied to
protect the private key. The way this password can be supplied may depend
on the application. If only one private key is handled, it can be practical
to have the callback handle the password dialog interactively. If several
keys have to be handled, it can be practical to ask for the password once,
then keep it in memory and use it several times. In the last case, the
password could be stored into the userdata storage and the
callback only returns the password already stored.

When asking for the password interactively, the callback can use
**rwflag** to check, whether an item shall be encrypted (rwflag=1).
In this case the password dialog may ask for the same password twice
for comparison in order to catch typos, that would make decryption
impossible.

Other items in PEM formatting (certificates) can also be encrypted, it is
however not usual, as certificate information is considered public.

# RETURN VALUES

These functions do not provide diagnostic information.

# EXAMPLES

The following example returns the password provided as userdata to the
calling function. The password is considered to be a '\\0' terminated
string. If the password does not fit into the buffer, the password is
truncated.

    int my_cb(char *buf, int size, int rwflag, void *u)
    {
         strncpy(buf, (char *)u, size);
         buf[size - 1] = '\0';
         return strlen(buf);
    }

# HISTORY

SSL\_CTX\_get\_default\_passwd\_cb(), SSL\_CTX\_get\_default\_passwd\_cb\_userdata(),
SSL\_set\_default\_passwd\_cb() and SSL\_set\_default\_passwd\_cb\_userdata() were
first added to OpenSSL 1.1.0

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[SSL\_CTX\_use\_certificate(3)](http://man.he.net/man3/SSL_CTX_use_certificate)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
