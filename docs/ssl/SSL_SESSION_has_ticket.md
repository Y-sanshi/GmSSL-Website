# NAME

SSL\_SESSION\_get0\_ticket,
SSL\_SESSION\_has\_ticket, SSL\_SESSION\_get\_ticket\_lifetime\_hint,
\- get details about the ticket associated with a session

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_SESSION_has_ticket(const SSL_SESSION *s);
    unsigned long SSL_SESSION_get_ticket_lifetime_hint(const SSL_SESSION *s);
    void SSL_SESSION_get0_ticket(const SSL_SESSION *s, const unsigned char **tick,
                                 size_t *len);

# DESCRIPTION

SSL\_SESSION\_has\_ticket() returns 1 if there is a Session Ticket associated with
this session, and 0 otherwise.

SSL\_SESSION\_get\_ticket\_lifetime\_hint returns the lifetime hint in seconds
associated with the session ticket.

SSL\_SESSION\_get0\_ticket obtains a pointer to the ticket associated with a
session. The length of the ticket is written to **\*len**. If **tick** is non
NULL then a pointer to the ticket is written to **\*tick**. The pointer is only
valid while the connection is in use. The session (and hence the ticket pointer)
may also become invalid as a result of a call to SSL\_CTX\_flush\_sessions().

# SEE ALSO

[ssl(3)](http://man.he.net/man3/ssl),
[d2i\_SSL\_SESSION(3)](http://man.he.net/man3/d2i_SSL_SESSION),
[SSL\_SESSION\_get\_time(3)](http://man.he.net/man3/SSL_SESSION_get_time),
[SSL\_SESSION\_free(3)](http://man.he.net/man3/SSL_SESSION_free)

# HISTORY

SSL\_SESSION\_has\_ticket, SSL\_SESSION\_get\_ticket\_lifetime\_hint and
SSL\_SESSION\_get0\_ticket were added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
