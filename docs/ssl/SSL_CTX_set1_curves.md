# NAME

SSL\_CTX\_set1\_curves, SSL\_CTX\_set1\_curves\_list, SSL\_set1\_curves,
SSL\_set1\_curves\_list, SSL\_get1\_curves, SSL\_get\_shared\_curve - EC supported curve functions

# SYNOPSIS

    #include <openssl/ssl.h>

    int SSL_CTX_set1_curves(SSL_CTX *ctx, int *clist, int clistlen);
    int SSL_CTX_set1_curves_list(SSL_CTX *ctx, char *list);

    int SSL_set1_curves(SSL *ssl, int *clist, int clistlen);
    int SSL_set1_curves_list(SSL *ssl, char *list);

    int SSL_get1_curves(SSL *ssl, int *curves);
    int SSL_get_shared_curve(SSL *s, int n);

# DESCRIPTION

SSL\_CTX\_set1\_curves() sets the supported curves for **ctx** to **clistlen**
curves in the array **clist**. The array consist of all NIDs of curves in
preference order. For a TLS client the curves are used directly in the
supported curves extension. For a TLS server the curves are used to
determine the set of shared curves.

SSL\_CTX\_set1\_curves\_list() sets the supported curves for **ctx** to
string **list**. The string is a colon separated list of curve NIDs or
names, for example "P-521:P-384:P-256".

SSL\_set1\_curves() and SSL\_set1\_curves\_list() are similar except they set
supported curves for the SSL structure **ssl**.

SSL\_get1\_curves() returns the set of supported curves sent by a client
in the supported curves extension. It returns the total number of
supported curves. The **curves** parameter can be **NULL** to simply
return the number of curves for memory allocation purposes. The
**curves** array is in the form of a set of curve NIDs in preference
order. It can return zero if the client did not send a supported curves
extension.

SSL\_get\_shared\_curve() returns shared curve **n** for a server-side
SSL **ssl**. If **n** is -1 then the total number of shared curves is
returned, which may be zero. Other than for diagnostic purposes,
most applications will only be interested in the first shared curve
so **n** is normally set to zero. If the value **n** is out of range,
NID\_undef is returned.

All these functions are implemented as macros.

# NOTES

If an application wishes to make use of several of these functions for
configuration purposes either on a command line or in a file it should
consider using the SSL\_CONF interface instead of manually parsing options.

# RETURN VALUES

SSL\_CTX\_set1\_curves(), SSL\_CTX\_set1\_curves\_list(), SSL\_set1\_curves() and
SSL\_set1\_curves\_list(), return 1 for success and 0 for failure.

SSL\_get1\_curves() returns the number of curves, which may be zero.

SSL\_get\_shared\_curve() returns the NID of shared curve **n** or NID\_undef if there
is no shared curve **n**; or the total number of shared curves if **n**
is -1.

When called on a client **ssl**, SSL\_get\_shared\_curve() has no meaning and
returns -1.

# SEE ALSO

[SSL\_CTX\_add\_extra\_chain\_cert(3)](http://man.he.net/man3/SSL_CTX_add_extra_chain_cert)

# HISTORY

These functions were first added to OpenSSL 1.0.2.

# COPYRIGHT

Copyright 2013-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
