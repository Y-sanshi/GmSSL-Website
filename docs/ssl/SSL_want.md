# NAME

SSL\_want, SSL\_want\_nothing, SSL\_want\_read, SSL\_want\_write, SSL\_want\_x509\_lookup,
SSL\_want\_async, SSL\_want\_async\_job - obtain state information TLS/SSL I/O
operation

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_want(const SSL *ssl);
    int SSL_want_nothing(const SSL *ssl);
    int SSL_want_read(const SSL *ssl);
    int SSL_want_write(const SSL *ssl);
    int SSL_want_x509_lookup(const SSL *ssl);
    int SSL_want_async(const SSL *ssl);
    int SSL_want_async_job(const SSL *ssl);

# DESCRIPTION

SSL\_want() returns state information for the SSL object **ssl**.

The other SSL\_want\_\*() calls are shortcuts for the possible states returned
by SSL\_want().

# NOTES

SSL\_want() examines the internal state information of the SSL object. Its
return values are similar to that of [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error).
Unlike [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error), which also evaluates the
error queue, the results are obtained by examining an internal state flag
only. The information must therefore only be used for normal operation under
non-blocking I/O. Error conditions are not handled and must be treated
using [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error).

The result returned by SSL\_want() should always be consistent with
the result of [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error).

# RETURN VALUES

The following return values can currently occur for SSL\_want():

- SSL\_NOTHING

    There is no data to be written or to be read.

- SSL\_WRITING

    There are data in the SSL buffer that must be written to the underlying
    **BIO** layer in order to complete the actual SSL\_\*() operation.
    A call to [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error) should return
    SSL\_ERROR\_WANT\_WRITE.

- SSL\_READING

    More data must be read from the underlying **BIO** layer in order to
    complete the actual SSL\_\*() operation.
    A call to [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error) should return
    SSL\_ERROR\_WANT\_READ.

- SSL\_X509\_LOOKUP

    The operation did not complete because an application callback set by
    SSL\_CTX\_set\_client\_cert\_cb() has asked to be called again.
    A call to [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error) should return
    SSL\_ERROR\_WANT\_X509\_LOOKUP.

- SSL\_ASYNC\_PAUSED

    An asynchronous operation partially completed and was then paused. See
    [SSL\_get\_all\_async\_fds(3)](http://man.he.net/man3/SSL_get_all_async_fds). A call to [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error) should return
    SSL\_ERROR\_WANT\_ASYNC.

- SSL\_ASYNC\_NO\_JOBS

    The asynchronous job could not be started because there were no async jobs
    available in the pool (see ASYNC\_init\_thread(3)). A call to [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error)
    should return SSL\_ERROR\_WANT\_ASYNC\_JOB.

SSL\_want\_nothing(), SSL\_want\_read(), SSL\_want\_write(), SSL\_want\_x509\_lookup(),
SSL\_want\_async() and SSL\_want\_async\_job() return 1, when the corresponding
condition is true or 0 otherwise.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [err(3)](http://man.he.net/man3/err), [SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error)

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
