# NAME

SSL\_CTX\_use\_psk\_identity\_hint, SSL\_use\_psk\_identity\_hint,
SSL\_CTX\_set\_psk\_server\_callback, SSL\_set\_psk\_server\_callback - set PSK
identity hint to use

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CTX_use_psk_identity_hint(SSL_CTX *ctx, const char *hint);
    int SSL_use_psk_identity_hint(SSL *ssl, const char *hint);

    void SSL_CTX_set_psk_server_callback(SSL_CTX *ctx,
           unsigned int (*callback)(SSL *ssl, const char *identity,
           unsigned char *psk, int max_psk_len));
    void SSL_set_psk_server_callback(SSL *ssl,
           unsigned int (*callback)(SSL *ssl, const char *identity,
           unsigned char *psk, int max_psk_len));

# DESCRIPTION

SSL\_CTX\_use\_psk\_identity\_hint() sets the given **NULL**-terminated PSK
identity hint **hint** to SSL context object
**ctx**. SSL\_use\_psk\_identity\_hint() sets the given **NULL**-terminated
PSK identity hint **hint** to SSL connection object **ssl**. If **hint**
is **NULL** the current hint from **ctx** or **ssl** is deleted.

In the case where PSK identity hint is **NULL**, the server
does not send the ServerKeyExchange message to the client.

A server application must provide a callback function which is called
when the server receives the ClientKeyExchange message from the
client. The purpose of the callback function is to validate the
received PSK identity and to fetch the pre-shared key used during the
connection setup phase. The callback is set using functions
SSL\_CTX\_set\_psk\_server\_callback() or
SSL\_set\_psk\_server\_callback(). The callback function is given the
connection in parameter **ssl**, **NULL**-terminated PSK identity sent
by the client in parameter **identity**, and a buffer **psk** of length
**max\_psk\_len** bytes where the pre-shared key is to be stored.

# RETURN VALUES

SSL\_CTX\_use\_psk\_identity\_hint() and SSL\_use\_psk\_identity\_hint() return
1 on success, 0 otherwise.

Return values from the server callback are interpreted as follows:

- 0

    PSK identity was not found. An "unknown\_psk\_identity" alert message
    will be sent and the connection setup fails.

- >0

    PSK identity was found and the server callback has provided the PSK
    successfully in parameter **psk**. Return value is the length of
    **psk** in bytes. It is an error to return a value greater than
    **max\_psk\_len**.

    If the PSK identity was not found but the callback instructs the
    protocol to continue anyway, the callback must provide some random
    data to **psk** and return the length of the random data, so the
    connection will fail with decryption\_error before it will be finished
    completely.

# COPYRIGHT

Copyright 2006-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).

Copyright 2005 Nokia.
