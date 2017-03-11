# NAME

SSL\_CTX\_set\_alpn\_protos, SSL\_set\_alpn\_protos, SSL\_CTX\_set\_alpn\_select\_cb,
SSL\_select\_next\_proto, SSL\_get0\_alpn\_selected - handle application layer
protocol negotiation (ALPN)

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CTX_set_alpn_protos(SSL_CTX *ctx, const unsigned char *protos,
                                unsigned int protos_len);
    int SSL_set_alpn_protos(SSL *ssl, const unsigned char *protos,
                            unsigned int protos_len);
    void SSL_CTX_set_alpn_select_cb(SSL_CTX *ctx,
                                    int (*cb) (SSL *ssl,
                                               const unsigned char **out,
                                               unsigned char *outlen,
                                               const unsigned char *in,
                                               unsigned int inlen,
                                               void *arg), void *arg);
    int SSL_select_next_proto(unsigned char **out, unsigned char *outlen,
                              const unsigned char *server,
                              unsigned int server_len,
                              const unsigned char *client,
                              unsigned int client_len)
    void SSL_get0_alpn_selected(const SSL *ssl, const unsigned char **data,
                                unsigned int *len);

# DESCRIPTION

SSL\_CTX\_set\_alpn\_protos() and SSL\_set\_alpn\_protos() are used by the client to
set the list of protocols available to be negotiated. The **protos** must be in
protocol-list format, described below. The length of **protos** is specified in
**protos\_len**.

SSL\_CTX\_set\_alpn\_select\_cb() sets the application callback **cb** used by a
server to select which protocol to use for the incoming connection. When **cb**
is NULL, ALPN is not used. The **arg** value is a pointer which is passed to
the application callback.

**cb** is the application defined callback. The **in**, **inlen** parameters are a
vector in protocol-list format. The value of the **out**, **outlen** vector
should be set to the value of a single protocol selected from the **in**,
**inlen** vector. The **out** buffer may point directly into **in**, or to a
buffer that outlives the handshake. The **arg** parameter is the pointer set via
SSL\_CTX\_set\_alpn\_select\_cb().

SSL\_select\_next\_proto() is a helper function used to select protocols. It
implements the standard protocol selection. It is expected that this function
is called from the application callback **cb**. The protocol data in **server**,
**server\_len** and **client**, **client\_len** must be in the protocol-list format
described below. The first item in the **server**, **server\_len** list that
matches an item in the **client**, **client\_len** list is selected, and returned
in **out**, **outlen**. The **out** value will point into either **server** or
**client**, so it should be copied immediately. If no match is found, the first
item in **client**, **client\_len** is returned in **out**, **outlen**. This
function can also be used in the NPN callback.

SSL\_get0\_alpn\_selected() returns a pointer to the selected protocol in **data**
with length **len**. It is not NUL-terminated. **data** is set to NULL and **len**
is set to 0 if no protocol has been selected. **data** must not be freed.

# NOTES

The protocol-lists must be in wire-format, which is defined as a vector of
non-empty, 8-bit length-prefixed, byte strings. The length-prefix byte is not
included in the length. Each string is limited to 255 bytes. A byte-string
length of 0 is invalid. A truncated byte-string is invalid. The length of the
vector is not in the vector itself, but in a separate variable.

Example:

    unsigned char vector[] = {
        6, 's', 'p', 'd', 'y', '/', '1',
        8, 'h', 't', 't', 'p', '/', '1', '.', '1'
    };
    unsigned int length = sizeof(vector);

The ALPN callback is executed after the servername callback; as that servername
callback may update the SSL\_CTX, and subsequently, the ALPN callback.

If there is no ALPN proposed in the ClientHello, the ALPN callback is not
invoked.

# RETURN VALUES

SSL\_CTX\_set\_alpn\_protos() and SSL\_set\_alpn\_protos() return 0 on success, and
non-0 on failure. WARNING: these functions reverse the return value convention.

SSL\_select\_next\_proto() returns one of the following:

- OPENSSL\_NPN\_NEGOTIATED

    A match was found and is returned in **out**, **outlen**.

- OPENSSL\_NPN\_NO\_OVERLAP

    No match was found. The first item in **client**, **client\_len** is returned in
    **out**, **outlen**.

The ALPN select callback **cb**, must return one of the following:

- SSL\_TLSEXT\_ERR\_OK

    ALPN protocol selected.

- SSL\_TLSEXT\_ERR\_NOACK

    ALPN protocol not selected.

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl), [SSL\_CTX\_set\_tlsext\_servername\_callback(3)](http://man.he.net/man3/SSL_CTX_set_tlsext_servername_callback),
[SSL\_CTX\_set\_tlsext\_servername\_arg(3)](http://man.he.net/man3/SSL_CTX_set_tlsext_servername_arg)

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
