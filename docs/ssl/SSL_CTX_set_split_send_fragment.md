# NAME

SSL\_CTX\_set\_max\_send\_fragment, SSL\_set\_max\_send\_fragment,
SSL\_CTX\_set\_split\_send\_fragment, SSL\_set\_split\_send\_fragment,
SSL\_CTX\_set\_max\_pipelines, SSL\_set\_max\_pipelines,
SSL\_CTX\_set\_default\_read\_buffer\_len, SSL\_set\_default\_read\_buffer\_len - Control
fragment sizes and pipelining operations

# SYNOPSIS

    #include <openssl/ssl.h>

    long SSL_CTX_set_max_send_fragment(SSL_CTX *ctx, long);
    long SSL_set_max_send_fragment(SSL *ssl, long m);

    long SSL_CTX_set_max_pipelines(SSL_CTX *ctx, long m);
    long SSL_set_max_pipelines(SSL_CTX *ssl, long m);

    long SSL_CTX_set_split_send_fragment(SSL_CTX *ctx, long m);
    long SSL_set_split_send_fragment(SSL *ssl, long m);

    void SSL_CTX_set_default_read_buffer_len(SSL_CTX *ctx, size_t len);
    void SSL_set_default_read_buffer_len(SSL *s, size_t len);

# DESCRIPTION

Some engines are able to process multiple simultaneous crypto operations. This
capability could be utilised to parallelise the processing of a single
connection. For example a single write can be split into multiple records and
each one encrypted independently and in parallel. Note: this will only work in
TLS1.1+. There is no support in SSLv3, TLSv1.0 or DTLS (any version). This
capability is known as "pipelining" within OpenSSL.

In order to benefit from the pipelining capability. You need to have an engine
that provides ciphers that support this. The OpenSSL "dasync" engine provides
AES128-SHA based ciphers that have this capability. However these are for
development and test purposes only.

SSL\_CTX\_set\_max\_send\_fragment() and SSL\_set\_max\_send\_fragment() set the
**max\_send\_fragment** parameter for SSL\_CTX and SSL objects respectively. This
value restricts the amount of plaintext bytes that will be sent in any one
SSL/TLS record. By default its value is SSL3\_RT\_MAX\_PLAIN\_LENGTH (16384). These
functions will only accept a value in the range 512 - SSL3\_RT\_MAX\_PLAIN\_LENGTH.

SSL\_CTX\_set\_max\_pipelines() and SSL\_set\_max\_pipelines() set the maximum number
of pipelines that will be used at any one time. This value applies to both
"read" pipelining and "write" pipelining. By default only one pipeline will be
used (i.e. normal non-parallel operation). The number of pipelines set must be
in the range 1 - SSL\_MAX\_PIPELINES (32). Setting this to a value > 1 will also
automatically turn on "read\_ahead" (see [SSL\_CTX\_set\_read\_ahead(3)](http://man.he.net/man3/SSL_CTX_set_read_ahead)). This is
explained further below. OpenSSL will only every use more than one pipeline if
a ciphersuite is negotiated that uses a pipeline capable cipher provided by an
engine.

Pipelining operates slightly differently for reading encrypted data compared to
writing encrypted data. SSL\_CTX\_set\_split\_send\_fragment() and
SSL\_set\_split\_send\_fragment() define how data is split up into pipelines when
writing encrypted data. The number of pipelines used will be determined by the
amount of data provided to the SSL\_write() call divided by
**split\_send\_fragment**.

For example if **split\_send\_fragment** is set to 2000 and **max\_pipelines** is 4
then:

SSL\_write called with 0-2000 bytes == 1 pipeline used

SSL\_write called with 2001-4000 bytes == 2 pipelines used

SSL\_write called with 4001-6000 bytes == 3 pipelines used

SSL\_write called with 6001+ bytes == 4 pipelines used

**split\_send\_fragment** must always be less than or equal to
**max\_send\_fragment**. By default it is set to be equal to **max\_send\_fragment**.
This will mean that the same number of records will always be created as would
have been created in the non-parallel case, although the data will be
apportioned differently. In the parallel case data will be spread equally
between the pipelines.

Read pipelining is controlled in a slightly different way than with write
pipelining. While reading we are constrained by the number of records that the
peer (and the network) can provide to us in one go. The more records we can get
in one go the more opportunity we have to parallelise the processing. As noted
above when setting **max\_pipelines** to a value greater than one, **read\_ahead**
is automatically set. The **read\_ahead** parameter causes OpenSSL to attempt to
read as much data into the read buffer as the network can provide and will fit
into the buffer. Without this set data is read into the read buffer one record
at a time. The more data that can be read, the more opportunity there is for
parallelising the processing at the cost of increased memory overhead per
connection. Setting **read\_ahead** can impact the behaviour of the SSL\_pending()
function (see [SSL\_pending(3)](http://man.he.net/man3/SSL_pending)).

The SSL\_CTX\_set\_default\_read\_buffer\_len() and SSL\_set\_default\_read\_buffer\_len()
functions control the size of the read buffer that will be used. The **len**
parameter sets the size of the buffer. The value will only be used if it is
greater than the default that would have been used anyway. The normal default
value depends on a number of factors but it will be at least
SSL3\_RT\_MAX\_PLAIN\_LENGTH + SSL3\_RT\_MAX\_ENCRYPTED\_OVERHEAD (16704) bytes.

# RETURN VALUES

All non-void functions return 1 on success and 0 on failure.

# NOTES

With the exception of SSL\_CTX\_set\_default\_read\_buffer\_len() and
SSL\_set\_default\_read\_buffer\_len() all these functions are implemented using
macros.

# HISTORY

The SSL\_CTX\_set\_max\_pipelines(), SSL\_set\_max\_pipelines(),
SSL\_CTX\_set\_split\_send\_fragment(), SSL\_set\_split\_send\_fragment(),
SSL\_CTX\_set\_default\_read\_buffer\_len() and  SSL\_set\_default\_read\_buffer\_len()
functions were added in OpenSSL 1.1.0.

# SEE ALSO

[SSL\_CTX\_set\_read\_ahead(3)](http://man.he.net/man3/SSL_CTX_set_read_ahead), [SSL\_pending(3)](http://man.he.net/man3/SSL_pending)

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
