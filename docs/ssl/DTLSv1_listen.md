# NAME

DTLSv1\_listen - listen for incoming DTLS connections

# SYNOPSIS

    #include <openssl/ssl.h>

    int DTLSv1_listen(SSL *ssl, BIO_ADDR *peer);

# DESCRIPTION

DTLSv1\_listen() listens for new incoming DTLS connections. If a ClientHello is
received that does not contain a cookie, then DTLSv1\_listen() responds with a
HelloVerifyRequest. If a ClientHello is received with a cookie that is verified
then control is returned to user code to enable the handshake to be completed
(for example by using SSL\_accept()).

# NOTES

Datagram based protocols can be susceptible to Denial of Service attacks. A
DTLS attacker could, for example, submit a series of handshake initiation
requests that cause the server to allocate state (and possibly perform
cryptographic operations) thus consuming server resources. The attacker could
also (with UDP) quite simply forge the source IP address in such an attack.

As a counter measure to that DTLS includes a stateless cookie mechanism. The
idea is that when a client attempts to connect to a server it sends a
ClientHello message. The server responds with a HelloVerifyRequest which
contains a unique cookie. The client then resends the ClientHello, but this time
includes the cookie in the message thus proving that the client is capable of
receiving messages sent to that address. All of this can be done by the server
without allocating any state, and thus without consuming expensive resources.

OpenSSL implements this capability via the DTLSv1\_listen() function. The **ssl**
parameter should be a newly allocated SSL object with its read and write BIOs
set, in the same way as might be done for a call to SSL\_accept(). Typically the
read BIO will be in an "unconnected" state and thus capable of receiving
messages from any peer.

When a ClientHello is received that contains a cookie that has been verified,
then DTLSv1\_listen() will return with the **ssl** parameter updated into a state
where the handshake can be continued by a call to (for example) SSL\_accept().
Additionally the **BIO\_ADDR** pointed to by **peer** will be filled in with
details of the peer that sent the ClientHello. If the underlying BIO is unable
to obtain the **BIO\_ADDR** of the peer (for example because the BIO does not
support this), then **\*peer** will be cleared and the family set to AF\_UNSPEC.
Typically user code is expected to "connect" the underlying socket to the peer
and continue the handshake in a connected state.

Prior to calling DTLSv1\_listen() user code must ensure that cookie generation
and verification callbacks have been set up using
SSL\_CTX\_set\_cookie\_generate\_cb() and SSL\_CTX\_set\_cookie\_verify\_cb()
respectively.

Since DTLSv1\_listen() operates entirely statelessly whilst processing incoming
ClientHellos it is unable to process fragmented messages (since this would
require the allocation of state). An implication of this is that DTLSv1\_listen()
**only** supports ClientHellos that fit inside a single datagram.

# RETURN VALUES

From OpenSSL 1.1.0 a return value of >= 1 indicates success. In this instance
the **peer** value will be filled in and the **ssl** object set up ready to
continue the handshake.

A return value of 0 indicates a non-fatal error. This could (for
example) be because of non-blocking IO, or some invalid message having been
received from a peer. Errors may be placed on the OpenSSL error queue with
further information if appropriate. Typically user code is expected to retry the
call to DTLSv1\_listen() in the event of a non-fatal error. Any old errors on the
error queue will be cleared in the subsequent call.

A return value of <0 indicates a fatal error. This could (for example) be
because of a failure to allocate sufficient memory for the operation.

Prior to OpenSSL 1.1.0 fatal and non-fatal errors both produce return codes
<= 0 (in typical implementations user code treats all errors as non-fatal),
whilst return codes >0 indicate success.

# SEE ALSO

[SSL\_get\_error(3)](http://man.he.net/man3/SSL_get_error), [SSL\_accept(3)](http://man.he.net/man3/SSL_accept),
[ssl(3)](http://man.he.net/man3/ssl), [bio(3)](http://man.he.net/man3/bio)

# HISTORY

DTLSv1\_listen() return codes were clarified in OpenSSL 1.1.0. The type of "peer"
also changed in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
