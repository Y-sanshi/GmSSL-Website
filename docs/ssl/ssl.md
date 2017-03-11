# NAME

SSL - OpenSSL SSL/TLS library

# SYNOPSIS

See the individual manual pages for details.

# DESCRIPTION

The OpenSSL **ssl** library implements the Secure Sockets Layer (SSL v2/v3) and
Transport Layer Security (TLS v1) protocols. It provides a rich API which is
documented here.

Then an **SSL\_CTX** object is created as a framework to establish
TLS/SSL enabled connections (see [SSL\_CTX\_new(3)](http://man.he.net/man3/SSL_CTX_new)).
Various options regarding certificates, algorithms etc. can be set
in this object.

When a network connection has been created, it can be assigned to an
**SSL** object. After the **SSL** object has been created using
[SSL\_new(3)](http://man.he.net/man3/SSL_new), [SSL\_set\_fd(3)](http://man.he.net/man3/SSL_set_fd) or
[SSL\_set\_bio(3)](http://man.he.net/man3/SSL_set_bio) can be used to associate the network
connection with the object.

Then the TLS/SSL handshake is performed using
[SSL\_accept(3)](http://man.he.net/man3/SSL_accept) or [SSL\_connect(3)](http://man.he.net/man3/SSL_connect)
respectively.
[SSL\_read(3)](http://man.he.net/man3/SSL_read) and [SSL\_write(3)](http://man.he.net/man3/SSL_write) are used
to read and write data on the TLS/SSL connection.
[SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown) can be used to shut down the
TLS/SSL connection.

# DATA STRUCTURES

Currently the OpenSSL **ssl** library functions deals with the following data
structures:

- **SSL\_METHOD** (SSL Method)

    That's a dispatch structure describing the internal **ssl** library
    methods/functions which implement the various protocol versions (SSLv3
    TLSv1, ...). It's needed to create an **SSL\_CTX**.

- **SSL\_CIPHER** (SSL Cipher)

    This structure holds the algorithm information for a particular cipher which
    are a core part of the SSL/TLS protocol. The available ciphers are configured
    on a **SSL\_CTX** basis and the actually used ones are then part of the
    **SSL\_SESSION**.

- **SSL\_CTX** (SSL Context)

    That's the global context structure which is created by a server or client
    once per program life-time and which holds mainly default values for the
    **SSL** structures which are later created for the connections.

- **SSL\_SESSION** (SSL Session)

    This is a structure containing the current TLS/SSL session details for a
    connection: **SSL\_CIPHER**s, client and server certificates, keys, etc.

- **SSL** (SSL Connection)

    That's the main SSL/TLS structure which is created by a server or client per
    established connection. This actually is the core structure in the SSL API.
    Under run-time the application usually deals with this structure which has
    links to mostly all other structures.

# HEADER FILES

Currently the OpenSSL **ssl** library provides the following C header files
containing the prototypes for the data structures and functions:

- **ssl.h**

    That's the common header file for the SSL/TLS API.  Include it into your
    program to make the API of the **ssl** library available. It internally
    includes both more private SSL headers and headers from the **crypto** library.
    Whenever you need hard-core details on the internals of the SSL API, look
    inside this header file.

- **ssl2.h**

    Unused. Present for backwards compatibility only.

- **ssl3.h**

    That's the sub header file dealing with the SSLv3 protocol only.
    _Usually you don't have to include it explicitly because
    it's already included by ssl.h_.

- **tls1.h**

    That's the sub header file dealing with the TLSv1 protocol only.
    _Usually you don't have to include it explicitly because
    it's already included by ssl.h_.

# API FUNCTIONS

Currently the OpenSSL **ssl** library exports 214 API functions.
They are documented in the following:

## Dealing with Protocol Methods

Here we document the various API functions which deal with the SSL/TLS
protocol methods defined in **SSL\_METHOD** structures.

- const SSL\_METHOD \***TLS\_method**(void);

    Constructor for the _version-flexible_ SSL\_METHOD structure for clients,
    servers or both.
    See [SSL\_CTX\_new(3)](http://man.he.net/man3/SSL_CTX_new) for details.

- const SSL\_METHOD \***TLS\_client\_method**(void);

    Constructor for the _version-flexible_ SSL\_METHOD structure for clients.

- const SSL\_METHOD \***TLS\_server\_method**(void);

    Constructor for the _version-flexible_ SSL\_METHOD structure for servers.

- const SSL\_METHOD \***TLSv1\_2\_method**(void);

    Constructor for the TLSv1.2 SSL\_METHOD structure for clients, servers or both.

- const SSL\_METHOD \***TLSv1\_2\_client\_method**(void);

    Constructor for the TLSv1.2 SSL\_METHOD structure for clients.

- const SSL\_METHOD \***TLSv1\_2\_server\_method**(void);

    Constructor for the TLSv1.2 SSL\_METHOD structure for servers.

- const SSL\_METHOD \***TLSv1\_1\_method**(void);

    Constructor for the TLSv1.1 SSL\_METHOD structure for clients, servers or both.

- const SSL\_METHOD \***TLSv1\_1\_client\_method**(void);

    Constructor for the TLSv1.1 SSL\_METHOD structure for clients.

- const SSL\_METHOD \***TLSv1\_1\_server\_method**(void);

    Constructor for the TLSv1.1 SSL\_METHOD structure for servers.

- const SSL\_METHOD \***TLSv1\_method**(void);

    Constructor for the TLSv1 SSL\_METHOD structure for clients, servers or both.

- const SSL\_METHOD \***TLSv1\_client\_method**(void);

    Constructor for the TLSv1 SSL\_METHOD structure for clients.

- const SSL\_METHOD \***TLSv1\_server\_method**(void);

    Constructor for the TLSv1 SSL\_METHOD structure for servers.

- const SSL\_METHOD \***SSLv3\_method**(void);

    Constructor for the SSLv3 SSL\_METHOD structure for clients, servers or both.

- const SSL\_METHOD \***SSLv3\_client\_method**(void);

    Constructor for the SSLv3 SSL\_METHOD structure for clients.

- const SSL\_METHOD \***SSLv3\_server\_method**(void);

    Constructor for the SSLv3 SSL\_METHOD structure for servers.

## Dealing with Ciphers

Here we document the various API functions which deal with the SSL/TLS
ciphers defined in **SSL\_CIPHER** structures.

- char \***SSL\_CIPHER\_description**(SSL\_CIPHER \*cipher, char \*buf, int len);

    Write a string to _buf_ (with a maximum size of _len_) containing a human
    readable description of _cipher_. Returns _buf_.

- int **SSL\_CIPHER\_get\_bits**(SSL\_CIPHER \*cipher, int \*alg\_bits);

    Determine the number of bits in _cipher_. Because of export crippled ciphers
    there are two bits: The bits the algorithm supports in general (stored to
    _alg\_bits_) and the bits which are actually used (the return value).

- const char \***SSL\_CIPHER\_get\_name**(SSL\_CIPHER \*cipher);

    Return the internal name of _cipher_ as a string. These are the various
    strings defined by the _SSL3\_TXT\_xxx_ and _TLS1\_TXT\_xxx_
    definitions in the header files.

- const char \***SSL\_CIPHER\_get\_version**(SSL\_CIPHER \*cipher);

    Returns a string like "`SSLv3`" or "`TLSv1.2`" which indicates the
    SSL/TLS protocol version to which _cipher_ belongs (i.e. where it was defined
    in the specification the first time).

## Dealing with Protocol Contexts

Here we document the various API functions which deal with the SSL/TLS
protocol context defined in the **SSL\_CTX** structure.

- int **SSL\_CTX\_add\_client\_CA**(SSL\_CTX \*ctx, X509 \*x);
- long **SSL\_CTX\_add\_extra\_chain\_cert**(SSL\_CTX \*ctx, X509 \*x509);
- int **SSL\_CTX\_add\_session**(SSL\_CTX \*ctx, SSL\_SESSION \*c);
- int **SSL\_CTX\_check\_private\_key**(const SSL\_CTX \*ctx);
- long **SSL\_CTX\_ctrl**(SSL\_CTX \*ctx, int cmd, long larg, char \*parg);
- void **SSL\_CTX\_flush\_sessions**(SSL\_CTX \*s, long t);
- void **SSL\_CTX\_free**(SSL\_CTX \*a);
- char \***SSL\_CTX\_get\_app\_data**(SSL\_CTX \*ctx);
- X509\_STORE \***SSL\_CTX\_get\_cert\_store**(SSL\_CTX \*ctx);
- STACK \***SSL\_CTX\_get\_ciphers**(const SSL\_CTX \*ctx);
- STACK \***SSL\_CTX\_get\_client\_CA\_list**(const SSL\_CTX \*ctx);
- int (\***SSL\_CTX\_get\_client\_cert\_cb**(SSL\_CTX \*ctx))(SSL \*ssl, X509 \*\*x509, EVP\_PKEY \*\*pkey);
- void **SSL\_CTX\_get\_default\_read\_ahead**(SSL\_CTX \*ctx);
- char \***SSL\_CTX\_get\_ex\_data**(const SSL\_CTX \*s, int idx);
- int **SSL\_CTX\_get\_ex\_new\_index**(long argl, char \*argp, int (\*new\_func);(void), int (\*dup\_func)(void), void (\*free\_func)(void))
- void (\***SSL\_CTX\_get\_info\_callback**(SSL\_CTX \*ctx))(SSL \*ssl, int cb, int ret);
- int **SSL\_CTX\_get\_quiet\_shutdown**(const SSL\_CTX \*ctx);
- void **SSL\_CTX\_get\_read\_ahead**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_get\_session\_cache\_mode**(SSL\_CTX \*ctx);
- long **SSL\_CTX\_get\_timeout**(const SSL\_CTX \*ctx);
- int (\***SSL\_CTX\_get\_verify\_callback**(const SSL\_CTX \*ctx))(int ok, X509\_STORE\_CTX \*ctx);
- int **SSL\_CTX\_get\_verify\_mode**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_load\_verify\_locations**(SSL\_CTX \*ctx, const char \*CAfile, const char \*CApath);
- SSL\_CTX \***SSL\_CTX\_new**(const SSL\_METHOD \*meth);
- int SSL\_CTX\_up\_ref(SSL\_CTX \*ctx);
- int **SSL\_CTX\_remove\_session**(SSL\_CTX \*ctx, SSL\_SESSION \*c);
- int **SSL\_CTX\_sess\_accept**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_sess\_accept\_good**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_sess\_accept\_renegotiate**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_sess\_cache\_full**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_sess\_cb\_hits**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_sess\_connect**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_sess\_connect\_good**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_sess\_connect\_renegotiate**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_sess\_get\_cache\_size**(SSL\_CTX \*ctx);
- SSL\_SESSION \*(\***SSL\_CTX\_sess\_get\_get\_cb**(SSL\_CTX \*ctx))(SSL \*ssl, unsigned char \*data, int len, int \*copy);
- int (\***SSL\_CTX\_sess\_get\_new\_cb**(SSL\_CTX \*ctx)(SSL \*ssl, SSL\_SESSION \*sess);
- void (\***SSL\_CTX\_sess\_get\_remove\_cb**(SSL\_CTX \*ctx)(SSL\_CTX \*ctx, SSL\_SESSION \*sess);
- int **SSL\_CTX\_sess\_hits**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_sess\_misses**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_sess\_number**(SSL\_CTX \*ctx);
- void **SSL\_CTX\_sess\_set\_cache\_size**(SSL\_CTX \*ctx, t);
- void **SSL\_CTX\_sess\_set\_get\_cb**(SSL\_CTX \*ctx, SSL\_SESSION \*(\*cb)(SSL \*ssl, unsigned char \*data, int len, int \*copy));
- void **SSL\_CTX\_sess\_set\_new\_cb**(SSL\_CTX \*ctx, int (\*cb)(SSL \*ssl, SSL\_SESSION \*sess));
- void **SSL\_CTX\_sess\_set\_remove\_cb**(SSL\_CTX \*ctx, void (\*cb)(SSL\_CTX \*ctx, SSL\_SESSION \*sess));
- int **SSL\_CTX\_sess\_timeouts**(SSL\_CTX \*ctx);
- LHASH \***SSL\_CTX\_sessions**(SSL\_CTX \*ctx);
- int **SSL\_CTX\_set\_app\_data**(SSL\_CTX \*ctx, void \*arg);
- void **SSL\_CTX\_set\_cert\_store**(SSL\_CTX \*ctx, X509\_STORE \*cs);
- void **SSL\_CTX\_set\_cert\_verify\_cb**(SSL\_CTX \*ctx, int (\*cb)(), char \*arg)
- int **SSL\_CTX\_set\_cipher\_list**(SSL\_CTX \*ctx, char \*str);
- void **SSL\_CTX\_set\_client\_CA\_list**(SSL\_CTX \*ctx, STACK \*list);
- void **SSL\_CTX\_set\_client\_cert\_cb**(SSL\_CTX \*ctx, int (\*cb)(SSL \*ssl, X509 \*\*x509, EVP\_PKEY \*\*pkey));
- int **SSL\_CTX\_set\_ct\_validation\_callback**(SSL\_CTX \*ctx, ssl\_ct\_validation\_cb callback, void \*arg);
- void **SSL\_CTX\_set\_default\_passwd\_cb**(SSL\_CTX \*ctx, int (\*cb);(void))
- void **SSL\_CTX\_set\_default\_read\_ahead**(SSL\_CTX \*ctx, int m);
- int **SSL\_CTX\_set\_default\_verify\_paths**(SSL\_CTX \*ctx);

    Use the default paths to locate trusted CA certificates. There is one default
    directory path and one default file path. Both are set via this call.

- int **SSL\_CTX\_set\_default\_verify\_dir**(SSL\_CTX \*ctx)

    Use the default directory path to locate trusted CA certificates.

- int **SSL\_CTX\_set\_default\_verify\_file**(SSL\_CTX \*ctx)

    Use the file path to locate trusted CA certificates.

- int **SSL\_CTX\_set\_ex\_data**(SSL\_CTX \*s, int idx, char \*arg);
- void **SSL\_CTX\_set\_info\_callback**(SSL\_CTX \*ctx, void (\*cb)(SSL \*ssl, int cb, int ret));
- void **SSL\_CTX\_set\_msg\_callback**(SSL\_CTX \*ctx, void (\*cb)(int write\_p, int version, int content\_type, const void \*buf, size\_t len, SSL \*ssl, void \*arg));
- void **SSL\_CTX\_set\_msg\_callback\_arg**(SSL\_CTX \*ctx, void \*arg);
- unsigned long **SSL\_CTX\_clear\_options**(SSL\_CTX \*ctx, unsigned long op);
- unsigned long **SSL\_CTX\_get\_options**(SSL\_CTX \*ctx);
- unsigned long **SSL\_CTX\_set\_options**(SSL\_CTX \*ctx, unsigned long op);
- void **SSL\_CTX\_set\_quiet\_shutdown**(SSL\_CTX \*ctx, int mode);
- void **SSL\_CTX\_set\_read\_ahead**(SSL\_CTX \*ctx, int m);
- void **SSL\_CTX\_set\_session\_cache\_mode**(SSL\_CTX \*ctx, int mode);
- int **SSL\_CTX\_set\_ssl\_version**(SSL\_CTX \*ctx, const SSL\_METHOD \*meth);
- void **SSL\_CTX\_set\_timeout**(SSL\_CTX \*ctx, long t);
- long **SSL\_CTX\_set\_tmp\_dh**(SSL\_CTX\* ctx, DH \*dh);
- long **SSL\_CTX\_set\_tmp\_dh\_callback**(SSL\_CTX \*ctx, DH \*(\*cb)(void));
- void **SSL\_CTX\_set\_verify**(SSL\_CTX \*ctx, int mode, int (\*cb);(void))
- int **SSL\_CTX\_use\_PrivateKey**(SSL\_CTX \*ctx, EVP\_PKEY \*pkey);
- int **SSL\_CTX\_use\_PrivateKey\_ASN1**(int type, SSL\_CTX \*ctx, unsigned char \*d, long len);
- int **SSL\_CTX\_use\_PrivateKey\_file**(SSL\_CTX \*ctx, const char \*file, int type);
- int **SSL\_CTX\_use\_RSAPrivateKey**(SSL\_CTX \*ctx, RSA \*rsa);
- int **SSL\_CTX\_use\_RSAPrivateKey\_ASN1**(SSL\_CTX \*ctx, unsigned char \*d, long len);
- int **SSL\_CTX\_use\_RSAPrivateKey\_file**(SSL\_CTX \*ctx, const char \*file, int type);
- int **SSL\_CTX\_use\_certificate**(SSL\_CTX \*ctx, X509 \*x);
- int **SSL\_CTX\_use\_certificate\_ASN1**(SSL\_CTX \*ctx, int len, unsigned char \*d);
- int **SSL\_CTX\_use\_certificate\_file**(SSL\_CTX \*ctx, const char \*file, int type);
- X509 \***SSL\_CTX\_get0\_certificate**(const SSL\_CTX \*ctx);
- EVP\_PKEY \***SSL\_CTX\_get0\_privatekey**(const SSL\_CTX \*ctx);
- void **SSL\_CTX\_set\_psk\_client\_callback**(SSL\_CTX \*ctx, unsigned int (\*callback)(SSL \*ssl, const char \*hint, char \*identity, unsigned int max\_identity\_len, unsigned char \*psk, unsigned int max\_psk\_len));
- int **SSL\_CTX\_use\_psk\_identity\_hint**(SSL\_CTX \*ctx, const char \*hint);
- void **SSL\_CTX\_set\_psk\_server\_callback**(SSL\_CTX \*ctx, unsigned int (\*callback)(SSL \*ssl, const char \*identity, unsigned char \*psk, int max\_psk\_len));

## Dealing with Sessions

Here we document the various API functions which deal with the SSL/TLS
sessions defined in the **SSL\_SESSION** structures.

- int **SSL\_SESSION\_cmp**(const SSL\_SESSION \*a, const SSL\_SESSION \*b);
- void **SSL\_SESSION\_free**(SSL\_SESSION \*ss);
- char \***SSL\_SESSION\_get\_app\_data**(SSL\_SESSION \*s);
- char \***SSL\_SESSION\_get\_ex\_data**(const SSL\_SESSION \*s, int idx);
- int **SSL\_SESSION\_get\_ex\_new\_index**(long argl, char \*argp, int (\*new\_func);(void), int (\*dup\_func)(void), void (\*free\_func)(void))
- long **SSL\_SESSION\_get\_time**(const SSL\_SESSION \*s);
- long **SSL\_SESSION\_get\_timeout**(const SSL\_SESSION \*s);
- unsigned long **SSL\_SESSION\_hash**(const SSL\_SESSION \*a);
- SSL\_SESSION \***SSL\_SESSION\_new**(void);
- int **SSL\_SESSION\_print**(BIO \*bp, const SSL\_SESSION \*x);
- int **SSL\_SESSION\_print\_fp**(FILE \*fp, const SSL\_SESSION \*x);
- int **SSL\_SESSION\_set\_app\_data**(SSL\_SESSION \*s, char \*a);
- int **SSL\_SESSION\_set\_ex\_data**(SSL\_SESSION \*s, int idx, char \*arg);
- long **SSL\_SESSION\_set\_time**(SSL\_SESSION \*s, long t);
- long **SSL\_SESSION\_set\_timeout**(SSL\_SESSION \*s, long t);

## Dealing with Connections

Here we document the various API functions which deal with the SSL/TLS
connection defined in the **SSL** structure.

- int **SSL\_accept**(SSL \*ssl);
- int **SSL\_add\_dir\_cert\_subjects\_to\_stack**(STACK \*stack, const char \*dir);
- int **SSL\_add\_file\_cert\_subjects\_to\_stack**(STACK \*stack, const char \*file);
- int **SSL\_add\_client\_CA**(SSL \*ssl, X509 \*x);
- char \***SSL\_alert\_desc\_string**(int value);
- char \***SSL\_alert\_desc\_string\_long**(int value);
- char \***SSL\_alert\_type\_string**(int value);
- char \***SSL\_alert\_type\_string\_long**(int value);
- int **SSL\_check\_private\_key**(const SSL \*ssl);
- void **SSL\_clear**(SSL \*ssl);
- long **SSL\_clear\_num\_renegotiations**(SSL \*ssl);
- int **SSL\_connect**(SSL \*ssl);
- int **SSL\_copy\_session\_id**(SSL \*t, const SSL \*f);

    Sets the session details for **t** to be the same as in **f**. Returns 1 on
    success or 0 on failure.

- long **SSL\_ctrl**(SSL \*ssl, int cmd, long larg, char \*parg);
- int **SSL\_do\_handshake**(SSL \*ssl);
- SSL \***SSL\_dup**(SSL \*ssl);

    SSL\_dup() allows applications to configure an SSL handle for use
    in multiple SSL connections, and then duplicate it prior to initiating
    each connection with the duplicated handle.
    Use of SSL\_dup() avoids the need to repeat the configuration of the
    handles for each connection.
    This is used internally by [BIO\_s\_accept(3)](http://man.he.net/man3/BIO_s_accept) to construct
    per-connection SSL handles after [accept(2)](http://man.he.net/man2/accept).

    For SSL\_dup() to work, the connection MUST be in its initial state
    and MUST NOT have not yet have started the SSL handshake.
    For connections that are not in their initial state SSL\_dup() just
    increments an internal reference count and returns the _same_
    handle.
    It may be possible to use [SSL\_clear(3)](http://man.he.net/man3/SSL_clear) to recycle an SSL handle
    that is not in its initial state for re-use, but this is best
    avoided.
    Instead, save and restore the session, if desired, and construct a
    fresh handle for each connection.

- STACK \***SSL\_dup\_CA\_list**(STACK \*sk);
- void **SSL\_free**(SSL \*ssl);
- SSL\_CTX \***SSL\_get\_SSL\_CTX**(const SSL \*ssl);
- char \***SSL\_get\_app\_data**(SSL \*ssl);
- X509 \***SSL\_get\_certificate**(const SSL \*ssl);
- const char \***SSL\_get\_cipher**(const SSL \*ssl);
- int **SSL\_is\_dtls**(const SSL \*ssl);
- int **SSL\_get\_cipher\_bits**(const SSL \*ssl, int \*alg\_bits);
- char \***SSL\_get\_cipher\_list**(const SSL \*ssl, int n);
- char \***SSL\_get\_cipher\_name**(const SSL \*ssl);
- char \***SSL\_get\_cipher\_version**(const SSL \*ssl);
- STACK \***SSL\_get\_ciphers**(const SSL \*ssl);
- STACK \***SSL\_get\_client\_CA\_list**(const SSL \*ssl);
- SSL\_CIPHER \***SSL\_get\_current\_cipher**(SSL \*ssl);
- long **SSL\_get\_default\_timeout**(const SSL \*ssl);
- int **SSL\_get\_error**(const SSL \*ssl, int i);
- char \***SSL\_get\_ex\_data**(const SSL \*ssl, int idx);
- int **SSL\_get\_ex\_data\_X509\_STORE\_CTX\_idx**(void);
- int **SSL\_get\_ex\_new\_index**(long argl, char \*argp, int (\*new\_func);(void), int (\*dup\_func)(void), void (\*free\_func)(void))
- int **SSL\_get\_fd**(const SSL \*ssl);
- void (\***SSL\_get\_info\_callback**(const SSL \*ssl);)()
- STACK \***SSL\_get\_peer\_cert\_chain**(const SSL \*ssl);
- X509 \***SSL\_get\_peer\_certificate**(const SSL \*ssl);
- const STACK\_OF(SCT) \***SSL\_get0\_peer\_scts**(SSL \*s);
- EVP\_PKEY \***SSL\_get\_privatekey**(const SSL \*ssl);
- int **SSL\_get\_quiet\_shutdown**(const SSL \*ssl);
- BIO \***SSL\_get\_rbio**(const SSL \*ssl);
- int **SSL\_get\_read\_ahead**(const SSL \*ssl);
- SSL\_SESSION \***SSL\_get\_session**(const SSL \*ssl);
- char \***SSL\_get\_shared\_ciphers**(const SSL \*ssl, char \*buf, int len);
- int **SSL\_get\_shutdown**(const SSL \*ssl);
- const SSL\_METHOD \***SSL\_get\_ssl\_method**(SSL \*ssl);
- int **SSL\_get\_state**(const SSL \*ssl);
- long **SSL\_get\_time**(const SSL \*ssl);
- long **SSL\_get\_timeout**(const SSL \*ssl);
- int (\***SSL\_get\_verify\_callback**(const SSL \*ssl))(int, X509\_STORE\_CTX \*)
- int **SSL\_get\_verify\_mode**(const SSL \*ssl);
- long **SSL\_get\_verify\_result**(const SSL \*ssl);
- char \***SSL\_get\_version**(const SSL \*ssl);
- BIO \***SSL\_get\_wbio**(const SSL \*ssl);
- int **SSL\_in\_accept\_init**(SSL \*ssl);
- int **SSL\_in\_before**(SSL \*ssl);
- int **SSL\_in\_connect\_init**(SSL \*ssl);
- int **SSL\_in\_init**(SSL \*ssl);
- int **SSL\_is\_init\_finished**(SSL \*ssl);
- STACK \***SSL\_load\_client\_CA\_file**(const char \*file);
- SSL \***SSL\_new**(SSL\_CTX \*ctx);
- int SSL\_up\_ref(SSL \*s);
- long **SSL\_num\_renegotiations**(SSL \*ssl);
- int **SSL\_peek**(SSL \*ssl, void \*buf, int num);
- int **SSL\_pending**(const SSL \*ssl);
- int **SSL\_read**(SSL \*ssl, void \*buf, int num);
- int **SSL\_renegotiate**(SSL \*ssl);
- char \***SSL\_rstate\_string**(SSL \*ssl);
- char \***SSL\_rstate\_string\_long**(SSL \*ssl);
- long **SSL\_session\_reused**(SSL \*ssl);
- void **SSL\_set\_accept\_state**(SSL \*ssl);
- void **SSL\_set\_app\_data**(SSL \*ssl, char \*arg);
- void **SSL\_set\_bio**(SSL \*ssl, BIO \*rbio, BIO \*wbio);
- int **SSL\_set\_cipher\_list**(SSL \*ssl, char \*str);
- void **SSL\_set\_client\_CA\_list**(SSL \*ssl, STACK \*list);
- void **SSL\_set\_connect\_state**(SSL \*ssl);
- int **SSL\_set\_ct\_validation\_callback**(SSL \*ssl, ssl\_ct\_validation\_cb callback, void \*arg);
- int **SSL\_set\_ex\_data**(SSL \*ssl, int idx, char \*arg);
- int **SSL\_set\_fd**(SSL \*ssl, int fd);
- void **SSL\_set\_info\_callback**(SSL \*ssl, void (\*cb);(void))
- void **SSL\_set\_msg\_callback**(SSL \*ctx, void (\*cb)(int write\_p, int version, int content\_type, const void \*buf, size\_t len, SSL \*ssl, void \*arg));
- void **SSL\_set\_msg\_callback\_arg**(SSL \*ctx, void \*arg);
- unsigned long **SSL\_clear\_options**(SSL \*ssl, unsigned long op);
- unsigned long **SSL\_get\_options**(SSL \*ssl);
- unsigned long **SSL\_set\_options**(SSL \*ssl, unsigned long op);
- void **SSL\_set\_quiet\_shutdown**(SSL \*ssl, int mode);
- void **SSL\_set\_read\_ahead**(SSL \*ssl, int yes);
- int **SSL\_set\_rfd**(SSL \*ssl, int fd);
- int **SSL\_set\_session**(SSL \*ssl, SSL\_SESSION \*session);
- void **SSL\_set\_shutdown**(SSL \*ssl, int mode);
- int **SSL\_set\_ssl\_method**(SSL \*ssl, const SSL\_METHOD \*meth);
- void **SSL\_set\_time**(SSL \*ssl, long t);
- void **SSL\_set\_timeout**(SSL \*ssl, long t);
- void **SSL\_set\_verify**(SSL \*ssl, int mode, int (\*callback);(void))
- void **SSL\_set\_verify\_result**(SSL \*ssl, long arg);
- int **SSL\_set\_wfd**(SSL \*ssl, int fd);
- int **SSL\_shutdown**(SSL \*ssl);
- OSSL\_HANDSHAKE\_STATE **SSL\_get\_state**(const SSL \*ssl);

    Returns the current handshake state.

- char \***SSL\_state\_string**(const SSL \*ssl);
- char \***SSL\_state\_string\_long**(const SSL \*ssl);
- long **SSL\_total\_renegotiations**(SSL \*ssl);
- int **SSL\_use\_PrivateKey**(SSL \*ssl, EVP\_PKEY \*pkey);
- int **SSL\_use\_PrivateKey\_ASN1**(int type, SSL \*ssl, unsigned char \*d, long len);
- int **SSL\_use\_PrivateKey\_file**(SSL \*ssl, const char \*file, int type);
- int **SSL\_use\_RSAPrivateKey**(SSL \*ssl, RSA \*rsa);
- int **SSL\_use\_RSAPrivateKey\_ASN1**(SSL \*ssl, unsigned char \*d, long len);
- int **SSL\_use\_RSAPrivateKey\_file**(SSL \*ssl, const char \*file, int type);
- int **SSL\_use\_certificate**(SSL \*ssl, X509 \*x);
- int **SSL\_use\_certificate\_ASN1**(SSL \*ssl, int len, unsigned char \*d);
- int **SSL\_use\_certificate\_file**(SSL \*ssl, const char \*file, int type);
- int **SSL\_version**(const SSL \*ssl);
- int **SSL\_want**(const SSL \*ssl);
- int **SSL\_want\_nothing**(const SSL \*ssl);
- int **SSL\_want\_read**(const SSL \*ssl);
- int **SSL\_want\_write**(const SSL \*ssl);
- int **SSL\_want\_x509\_lookup**(const SSL \*ssl);
- int **SSL\_write**(SSL \*ssl, const void \*buf, int num);
- void **SSL\_set\_psk\_client\_callback**(SSL \*ssl, unsigned int (\*callback)(SSL \*ssl, const char \*hint, char \*identity, unsigned int max\_identity\_len, unsigned char \*psk, unsigned int max\_psk\_len));
- int **SSL\_use\_psk\_identity\_hint**(SSL \*ssl, const char \*hint);
- void **SSL\_set\_psk\_server\_callback**(SSL \*ssl, unsigned int (\*callback)(SSL \*ssl, const char \*identity, unsigned char \*psk, int max\_psk\_len));
- const char \***SSL\_get\_psk\_identity\_hint**(SSL \*ssl);
- const char \***SSL\_get\_psk\_identity**(SSL \*ssl);

# RETURN VALUES

See the individual manual pages for details.

# SEE ALSO

[openssl(1)](http://man.he.net/man1/openssl), [crypto(3)](http://man.he.net/man3/crypto),
[CRYPTO\_get\_ex\_new\_index(3)](http://man.he.net/man3/CRYPTO_get_ex_new_index),
[SSL\_accept(3)](http://man.he.net/man3/SSL_accept), [SSL\_clear(3)](http://man.he.net/man3/SSL_clear),
[SSL\_connect(3)](http://man.he.net/man3/SSL_connect),
[SSL\_CIPHER\_get\_name(3)](http://man.he.net/man3/SSL_CIPHER_get_name),
[SSL\_COMP\_add\_compression\_method(3)](http://man.he.net/man3/SSL_COMP_add_compression_method),
[SSL\_CTX\_add\_extra\_chain\_cert(3)](http://man.he.net/man3/SSL_CTX_add_extra_chain_cert),
[SSL\_CTX\_add\_session(3)](http://man.he.net/man3/SSL_CTX_add_session),
[SSL\_CTX\_ctrl(3)](http://man.he.net/man3/SSL_CTX_ctrl),
[SSL\_CTX\_flush\_sessions(3)](http://man.he.net/man3/SSL_CTX_flush_sessions),
[SSL\_CTX\_get\_verify\_mode(3)](http://man.he.net/man3/SSL_CTX_get_verify_mode),
[SSL\_CTX\_load\_verify\_locations(3)](http://man.he.net/man3/SSL_CTX_load_verify_locations)
[SSL\_CTX\_new(3)](http://man.he.net/man3/SSL_CTX_new),
[SSL\_CTX\_sess\_number(3)](http://man.he.net/man3/SSL_CTX_sess_number),
[SSL\_CTX\_sess\_set\_cache\_size(3)](http://man.he.net/man3/SSL_CTX_sess_set_cache_size),
[SSL\_CTX\_sess\_set\_get\_cb(3)](http://man.he.net/man3/SSL_CTX_sess_set_get_cb),
[SSL\_CTX\_sessions(3)](http://man.he.net/man3/SSL_CTX_sessions),
[SSL\_CTX\_set\_cert\_store(3)](http://man.he.net/man3/SSL_CTX_set_cert_store),
[SSL\_CTX\_set\_cert\_verify\_callback(3)](http://man.he.net/man3/SSL_CTX_set_cert_verify_callback),
[SSL\_CTX\_set\_cipher\_list(3)](http://man.he.net/man3/SSL_CTX_set_cipher_list),
[SSL\_CTX\_set\_client\_CA\_list(3)](http://man.he.net/man3/SSL_CTX_set_client_CA_list),
[SSL\_CTX\_set\_client\_cert\_cb(3)](http://man.he.net/man3/SSL_CTX_set_client_cert_cb),
[SSL\_CTX\_set\_default\_passwd\_cb(3)](http://man.he.net/man3/SSL_CTX_set_default_passwd_cb),
[SSL\_CTX\_set\_generate\_session\_id(3)](http://man.he.net/man3/SSL_CTX_set_generate_session_id),
[SSL\_CTX\_set\_info\_callback(3)](http://man.he.net/man3/SSL_CTX_set_info_callback),
[SSL\_CTX\_set\_max\_cert\_list(3)](http://man.he.net/man3/SSL_CTX_set_max_cert_list),
[SSL\_CTX\_set\_mode(3)](http://man.he.net/man3/SSL_CTX_set_mode),
[SSL\_CTX\_set\_msg\_callback(3)](http://man.he.net/man3/SSL_CTX_set_msg_callback),
[SSL\_CTX\_set\_options(3)](http://man.he.net/man3/SSL_CTX_set_options),
[SSL\_CTX\_set\_quiet\_shutdown(3)](http://man.he.net/man3/SSL_CTX_set_quiet_shutdown),
[SSL\_CTX\_set\_read\_ahead(3)](http://man.he.net/man3/SSL_CTX_set_read_ahead),
[SSL\_CTX\_set\_security\_level(3)](http://man.he.net/man3/SSL_CTX_set_security_level),
[SSL\_CTX\_set\_session\_cache\_mode(3)](http://man.he.net/man3/SSL_CTX_set_session_cache_mode),
[SSL\_CTX\_set\_session\_id\_context(3)](http://man.he.net/man3/SSL_CTX_set_session_id_context),
[SSL\_CTX\_set\_ssl\_version(3)](http://man.he.net/man3/SSL_CTX_set_ssl_version),
[SSL\_CTX\_set\_timeout(3)](http://man.he.net/man3/SSL_CTX_set_timeout),
[SSL\_CTX\_set\_tmp\_dh\_callback(3)](http://man.he.net/man3/SSL_CTX_set_tmp_dh_callback),
[SSL\_CTX\_set\_verify(3)](http://man.he.net/man3/SSL_CTX_set_verify),
[SSL\_CTX\_use\_certificate(3)](http://man.he.net/man3/SSL_CTX_use_certificate),
[SSL\_alert\_type\_string(3)](http://man.he.net/man3/SSL_alert_type_string),
[SSL\_do\_handshake(3)](http://man.he.net/man3/SSL_do_handshake),
[SSL\_enable\_ct(3)](http://man.he.net/man3/SSL_enable_ct),
[SSL\_get\_SSL\_CTX(3)](http://man.he.net/man3/SSL_get_SSL_CTX),
[SSL\_get\_ciphers(3)](http://man.he.net/man3/SSL_get_ciphers),
[SSL\_get\_client\_CA\_list(3)](http://man.he.net/man3/SSL_get_client_CA_list),
[SSL\_get\_default\_timeout(3)](http://man.he.net/man3/SSL_get_default_timeout),
[SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error),
[SSL\_get\_ex\_data\_X509\_STORE\_CTX\_idx(3)](http://man.he.net/man3/SSL_get_ex_data_X509_STORE_CTX_idx),
[SSL\_get\_fd(3)](http://man.he.net/man3/SSL_get_fd),
[SSL\_get\_peer\_cert\_chain(3)](http://man.he.net/man3/SSL_get_peer_cert_chain),
[SSL\_get\_rbio(3)](http://man.he.net/man3/SSL_get_rbio),
[SSL\_get\_session(3)](http://man.he.net/man3/SSL_get_session),
[SSL\_get\_verify\_result(3)](http://man.he.net/man3/SSL_get_verify_result),
[SSL\_get\_version(3)](http://man.he.net/man3/SSL_get_version),
[SSL\_load\_client\_CA\_file(3)](http://man.he.net/man3/SSL_load_client_CA_file),
[SSL\_new(3)](http://man.he.net/man3/SSL_new),
[SSL\_pending(3)](http://man.he.net/man3/SSL_pending),
[SSL\_read(3)](http://man.he.net/man3/SSL_read),
[SSL\_rstate\_string(3)](http://man.he.net/man3/SSL_rstate_string),
[SSL\_session\_reused(3)](http://man.he.net/man3/SSL_session_reused),
[SSL\_set\_bio(3)](http://man.he.net/man3/SSL_set_bio),
[SSL\_set\_connect\_state(3)](http://man.he.net/man3/SSL_set_connect_state),
[SSL\_set\_fd(3)](http://man.he.net/man3/SSL_set_fd),
[SSL\_set\_session(3)](http://man.he.net/man3/SSL_set_session),
[SSL\_set\_shutdown(3)](http://man.he.net/man3/SSL_set_shutdown),
[SSL\_shutdown(3)](http://man.he.net/man3/SSL_shutdown),
[SSL\_state\_string(3)](http://man.he.net/man3/SSL_state_string),
[SSL\_want(3)](http://man.he.net/man3/SSL_want),
[SSL\_write(3)](http://man.he.net/man3/SSL_write),
[SSL\_SESSION\_free(3)](http://man.he.net/man3/SSL_SESSION_free),
[SSL\_SESSION\_get\_time(3)](http://man.he.net/man3/SSL_SESSION_get_time),
[d2i\_SSL\_SESSION(3)](http://man.he.net/man3/d2i_SSL_SESSION),
[SSL\_CTX\_set\_psk\_client\_callback(3)](http://man.he.net/man3/SSL_CTX_set_psk_client_callback),
[SSL\_CTX\_use\_psk\_identity\_hint(3)](http://man.he.net/man3/SSL_CTX_use_psk_identity_hint),
[SSL\_get\_psk\_identity(3)](http://man.he.net/man3/SSL_get_psk_identity),
[DTLSv1\_listen(3)](http://man.he.net/man3/DTLSv1_listen)

# HISTORY

**SSLv2\_client\_method**, **SSLv2\_server\_method** and **SSLv2\_method** where removed
in OpenSSL 1.1.0.

The return type of **SSL\_copy\_session\_id** was changed from void to int in
OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
