# NAME

SSL\_set\_bio, SSL\_set0\_rbio, SSL\_set0\_wbio - connect the SSL object with a BIO

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_set_bio(SSL *ssl, BIO *rbio, BIO *wbio);
    void SSL_set0_rbio(SSL *s, BIO *rbio);
    void SSL_set0_wbio(SSL *s, BIO *wbio);

# DESCRIPTION

SSL\_set0\_rbio() connects the BIO **rbio** for the read operations of the **ssl**
object. The SSL engine inherits the behaviour of **rbio**. If the BIO is
non-blocking then the **ssl** object will also have non-blocking behaviour. This
function transfers ownership of **rbio** to **ssl**. It will be automatically
freed using [BIO\_free\_all(3)](http://man.he.net/man3/BIO_free_all) when the **ssl** is freed. On calling this
function, any existing **rbio** that was previously set will also be freed via a
call to [BIO\_free\_all(3)](http://man.he.net/man3/BIO_free_all) (this includes the case where the **rbio** is set to
the same value as previously).

SSL\_set0\_wbio() works in the same as SSL\_set0\_rbio() except that it connects
the BIO **wbio** for the write operations of the **ssl** object. Note that if the
rbio and wbio are the same then SSL\_set0\_rbio() and SSL\_set0\_wbio() each take
ownership of one reference. Therefore it may be necessary to increment the
number of references available using [BIO\_up\_ref(3)](http://man.he.net/man3/BIO_up_ref) before calling the set0
functions.

SSL\_set\_bio() does a similar job as SSL\_set0\_rbio() and SSL\_set0\_wbio() except
that it connects both the **rbio** and the **wbio** at the same time. This
function transfers the ownership of **rbio** and **wbio** to **ssl** except that
the rules for this are much more complex. For this reason this function is
considered a legacy function and SSL\_set0\_rbio() and SSL\_set0\_wbio() should be
used in preference. The ownership rules are as follows:

- If neither the rbio or wbio have changed from their previous values then nothing
is done.
- If the rbio and wbio parameters are different and both are different to their
previously set values then one reference is consumed for the rbio and one
reference is consumed for the wbio.
- If the rbio and wbio parameters are the same and the rbio is not the same as the
previously set value then one reference is consumed.
- If the rbio and wbio parameters are the same and the rbio is the same as the
previously set value, then no additional references are consumed.
- If the rbio and wbio parameters are different and the rbio is the same as the
previously set value then one reference is consumbed for the wbio and no
references are consumed for the rbio.
- If the rbio and wbio parameters are different and the wbio is the same as the
previously set value and the old rbio and wbio values were the same as each
other then one reference is consumed for the rbio and no references are consumed
for the wbio.
- If the rbio and wbio parameters are different and the wbio is the same as the
previously set value and the old rbio and wbio values were different to each
other then one reference is consumed for the rbio and one reference is consumed
for the wbio.

# RETURN VALUES

SSL\_set\_bio(), SSL\_set\_rbio() and SSL\_set\_wbio() cannot fail.

# SEE ALSO

[SSL\_get\_rbio(3)](http://man.he.net/man3/SSL_get_rbio),
[SSL\_connect(3)](http://man.he.net/man3/SSL_connect), [SSL\_accept(3)](http://man.he.net/man3/SSL_accept),
[SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown), [ssl(3)](http://man.he.net/man3/ssl), [bio(3)](http://man.he.net/man3/bio)

# HISTORY

SSL\_set0\_rbio() and SSL\_set0\_wbio() were added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
