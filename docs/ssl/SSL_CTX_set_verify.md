# NAME

SSL\_CTX\_set\_verify, SSL\_set\_verify,
SSL\_CTX\_set\_verify\_depth, SSL\_set\_verify\_depth,
SSL\_verify\_cb
\- set peer certificate verification parameters

# SYNOPSIS

    #include <openssl/ssl.h>

    void SSL_CTX_set_verify(SSL_CTX *ctx, int mode, SSL_verify_cb verify_callback);
    void SSL_set_verify(SSL *s, int mode, SSL_verify_cb verify_callback);
    void SSL_CTX_set_verify_depth(SSL_CTX *ctx, int depth);
    void SSL_set_verify_depth(SSL *s, int depth);

    typedef int (*SSL_verify_cb)(int preverify_ok, X509_STORE_CTX *x509_ctx);

# DESCRIPTION

SSL\_CTX\_set\_verify() sets the verification flags for **ctx** to be **mode** and
specifies the **verify\_callback** function to be used. If no callback function
shall be specified, the NULL pointer can be used for **verify\_callback**.

SSL\_set\_verify() sets the verification flags for **ssl** to be **mode** and
specifies the **verify\_callback** function to be used. If no callback function
shall be specified, the NULL pointer can be used for **verify\_callback**. In
this case last **verify\_callback** set specifically for this **ssl** remains. If
no special **callback** was set before, the default callback for the underlying
**ctx** is used, that was valid at the time **ssl** was created with
[SSL\_new(3)](http://man.he.net/man3/SSL_new).

SSL\_CTX\_set\_verify\_depth() sets the maximum **depth** for the certificate chain
verification that shall be allowed for **ctx**. (See the BUGS section.)

SSL\_set\_verify\_depth() sets the maximum **depth** for the certificate chain
verification that shall be allowed for **ssl**. (See the BUGS section.)

# NOTES

The verification of certificates can be controlled by a set of logically
or'ed **mode** flags:

- SSL\_VERIFY\_NONE

    **Server mode:** the server will not send a client certificate request to the
    client, so the client will not send a certificate.

    **Client mode:** if not using an anonymous cipher (by default disabled), the
    server will send a certificate which will be checked. The result of the
    certificate verification process can be checked after the TLS/SSL handshake
    using the [SSL\_get\_verify\_result(3)](http://man.he.net/man3/SSL_get_verify_result) function.
    The handshake will be continued regardless of the verification result.

- SSL\_VERIFY\_PEER

    **Server mode:** the server sends a client certificate request to the client.
    The certificate returned (if any) is checked. If the verification process
    fails, the TLS/SSL handshake is
    immediately terminated with an alert message containing the reason for
    the verification failure.
    The behaviour can be controlled by the additional
    SSL\_VERIFY\_FAIL\_IF\_NO\_PEER\_CERT and SSL\_VERIFY\_CLIENT\_ONCE flags.

    **Client mode:** the server certificate is verified. If the verification process
    fails, the TLS/SSL handshake is
    immediately terminated with an alert message containing the reason for
    the verification failure. If no server certificate is sent, because an
    anonymous cipher is used, SSL\_VERIFY\_PEER is ignored.

- SSL\_VERIFY\_FAIL\_IF\_NO\_PEER\_CERT

    **Server mode:** if the client did not return a certificate, the TLS/SSL
    handshake is immediately terminated with a "handshake failure" alert.
    This flag must be used together with SSL\_VERIFY\_PEER.

    **Client mode:** ignored

- SSL\_VERIFY\_CLIENT\_ONCE

    **Server mode:** only request a client certificate on the initial TLS/SSL
    handshake. Do not ask for a client certificate again in case of a
    renegotiation. This flag must be used together with SSL\_VERIFY\_PEER.

    **Client mode:** ignored

If the **mode** is SSL\_VERIFY\_NONE none of the other flags may be set.

The actual verification procedure is performed either using the built-in
verification procedure or using another application provided verification
function set with
[SSL\_CTX\_set\_cert\_verify\_callback(3)](http://man.he.net/man3/SSL_CTX_set_cert_verify_callback).
The following descriptions apply in the case of the built-in procedure. An
application provided procedure also has access to the verify depth information
and the verify\_callback() function, but the way this information is used
may be different.

SSL\_CTX\_set\_verify\_depth() and SSL\_set\_verify\_depth() set the limit up
to which depth certificates in a chain are used during the verification
procedure. If the certificate chain is longer than allowed, the certificates
above the limit are ignored. Error messages are generated as if these
certificates would not be present, most likely a
X509\_V\_ERR\_UNABLE\_TO\_GET\_ISSUER\_CERT\_LOCALLY will be issued.
The depth count is "level 0:peer certificate", "level 1: CA certificate",
"level 2: higher level CA certificate", and so on. Setting the maximum
depth to 2 allows the levels 0, 1, and 2. The default depth limit is 100,
allowing for the peer certificate and additional 100 CA certificates.

The **verify\_callback** function is used to control the behaviour when the
SSL\_VERIFY\_PEER flag is set. It must be supplied by the application and
receives two arguments: **preverify\_ok** indicates, whether the verification of
the certificate in question was passed (preverify\_ok=1) or not
(preverify\_ok=0). **x509\_ctx** is a pointer to the complete context used
for the certificate chain verification.

The certificate chain is checked starting with the deepest nesting level
(the root CA certificate) and worked upward to the peer's certificate.
At each level signatures and issuer attributes are checked. Whenever
a verification error is found, the error number is stored in **x509\_ctx**
and **verify\_callback** is called with **preverify\_ok**=0. By applying
X509\_CTX\_store\_\* functions **verify\_callback** can locate the certificate
in question and perform additional steps (see EXAMPLES). If no error is
found for a certificate, **verify\_callback** is called with **preverify\_ok**=1
before advancing to the next level.

The return value of **verify\_callback** controls the strategy of the further
verification process. If **verify\_callback** returns 0, the verification
process is immediately stopped with "verification failed" state. If
SSL\_VERIFY\_PEER is set, a verification failure alert is sent to the peer and
the TLS/SSL handshake is terminated. If **verify\_callback** returns 1,
the verification process is continued. If **verify\_callback** always returns
1, the TLS/SSL handshake will not be terminated with respect to verification
failures and the connection will be established. The calling process can
however retrieve the error code of the last verification error using
[SSL\_get\_verify\_result(3)](http://man.he.net/man3/SSL_get_verify_result) or by maintaining its
own error storage managed by **verify\_callback**.

If no **verify\_callback** is specified, the default callback will be used.
Its return value is identical to **preverify\_ok**, so that any verification
failure will lead to a termination of the TLS/SSL handshake with an
alert message, if SSL\_VERIFY\_PEER is set.

# BUGS

In client mode, it is not checked whether the SSL\_VERIFY\_PEER flag
is set, but whether any flags are set. This can lead to
unexpected behaviour if SSL\_VERIFY\_PEER and other flags are not used as
required.

# RETURN VALUES

The SSL\*\_set\_verify\*() functions do not provide diagnostic information.

# EXAMPLES

The following code sequence realizes an example **verify\_callback** function
that will always continue the TLS/SSL handshake regardless of verification
failure, if wished. The callback realizes a verification depth limit with
more informational output.

All verification errors are printed; information about the certificate chain
is printed on request.
The example is realized for a server that does allow but not require client
certificates.

The example makes use of the ex\_data technique to store application data
into/retrieve application data from the SSL structure
(see [SSL\_get\_ex\_new\_index(3)](http://man.he.net/man3/SSL_get_ex_new_index),
[SSL\_get\_ex\_data\_X509\_STORE\_CTX\_idx(3)](http://man.he.net/man3/SSL_get_ex_data_X509_STORE_CTX_idx)).

    ...
    typedef struct {
      int verbose_mode;
      int verify_depth;
      int always_continue;
    } mydata_t;
    int mydata_index;
    ...
    static int verify_callback(int preverify_ok, X509_STORE_CTX *ctx)
    {
       char    buf[256];
       X509   *err_cert;
       int     err, depth;
       SSL    *ssl;
       mydata_t *mydata;

       err_cert = X509_STORE_CTX_get_current_cert(ctx);
       err = X509_STORE_CTX_get_error(ctx);
       depth = X509_STORE_CTX_get_error_depth(ctx);

       /*
        * Retrieve the pointer to the SSL of the connection currently treated
        * and the application specific data stored into the SSL object.
        */
       ssl = X509_STORE_CTX_get_ex_data(ctx, SSL_get_ex_data_X509_STORE_CTX_idx());
       mydata = SSL_get_ex_data(ssl, mydata_index);

       X509_NAME_oneline(X509_get_subject_name(err_cert), buf, 256);

       /*
        * Catch a too long certificate chain. The depth limit set using
        * SSL_CTX_set_verify_depth() is by purpose set to "limit+1" so
        * that whenever the "depth>verify_depth" condition is met, we
        * have violated the limit and want to log this error condition.
        * We must do it here, because the CHAIN_TOO_LONG error would not
        * be found explicitly; only errors introduced by cutting off the
        * additional certificates would be logged.
        */
       if (depth > mydata->verify_depth) {
           preverify_ok = 0;
           err = X509_V_ERR_CERT_CHAIN_TOO_LONG;
           X509_STORE_CTX_set_error(ctx, err);
       }
       if (!preverify_ok) {
           printf("verify error:num=%d:%s:depth=%d:%s\n", err,
                    X509_verify_cert_error_string(err), depth, buf);
       }
       else if (mydata->verbose_mode)
       {
           printf("depth=%d:%s\n", depth, buf);
       }

       /*
        * At this point, err contains the last verification error. We can use
        * it for something special
        */
       if (!preverify_ok && (err == X509_V_ERR_UNABLE_TO_GET_ISSUER_CERT))
       {
         X509_NAME_oneline(X509_get_issuer_name(err_cert), buf, 256);
         printf("issuer= %s\n", buf);
       }

       if (mydata->always_continue)
         return 1;
       else
         return preverify_ok;
    }
    ...

    mydata_t mydata;

    ...
    mydata_index = SSL_get_ex_new_index(0, "mydata index", NULL, NULL, NULL);

    ...
    SSL_CTX_set_verify(ctx, SSL_VERIFY_PEER|SSL_VERIFY_CLIENT_ONCE,
                       verify_callback);

    /*
     * Let the verify_callback catch the verify_depth error so that we get
     * an appropriate error in the logfile.
     */
    SSL_CTX_set_verify_depth(verify_depth + 1);

    /*
     * Set up the SSL specific data into "mydata" and store it into th SSL
     * structure.
     */
    mydata.verify_depth = verify_depth; ...
    SSL_set_ex_data(ssl, mydata_index, &mydata);

    ...
    SSL_accept(ssl);       /* check of success left out for clarity */
    if (peer = SSL_get_peer_certificate(ssl))
    {
      if (SSL_get_verify_result(ssl) == X509_V_OK)
      {
        /* The client sent a certificate which verified OK */
      }
    }

# SEE ALSO

[ssl(7)](http://man.he.net/man7/ssl), [SSL\_new(3)](http://man.he.net/man3/SSL_new),
[SSL\_CTX\_get\_verify\_mode(3)](http://man.he.net/man3/SSL_CTX_get_verify_mode),
[SSL\_get\_verify\_result(3)](http://man.he.net/man3/SSL_get_verify_result),
[SSL\_CTX\_load\_verify\_locations(3)](http://man.he.net/man3/SSL_CTX_load_verify_locations),
[SSL\_get\_peer\_certificate(3)](http://man.he.net/man3/SSL_get_peer_certificate),
[SSL\_CTX\_set\_cert\_verify\_callback(3)](http://man.he.net/man3/SSL_CTX_set_cert_verify_callback),
[SSL\_get\_ex\_data\_X509\_STORE\_CTX\_idx(3)](http://man.he.net/man3/SSL_get_ex_data_X509_STORE_CTX_idx),
[SSL\_get\_ex\_new\_index(3)](http://man.he.net/man3/SSL_get_ex_new_index)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
