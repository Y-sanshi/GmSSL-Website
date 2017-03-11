# NAME

ct - Certificate Transparency

# SYNOPSIS

    #include <openssl/ct.h>

# DESCRIPTION

This library implements Certificate Transparency (CT) verification for TLS
clients, as defined in RFC 6962. This verification can provide some confidence
that a certificate has been publicly logged in a set of CT logs.

By default, these checks are disabled. They can be enabled using
SSL\_CTX\_ct\_enable() or SSL\_ct\_enable().

This library can also be used to parse and examine CT data structures, such as
Signed Certificate Timestamps (SCTs), or to read a list of CT logs. There are
functions for:
\- decoding and encoding SCTs in DER and TLS wire format.
\- printing SCTs.
\- verifying the authenticity of SCTs.
\- loading a CT log list from a CONF file.

# SEE ALSO

[d2i\_SCT\_LIST(3)](http://man.he.net/man3/d2i_SCT_LIST),
[CTLOG\_STORE\_new(3)](http://man.he.net/man3/CTLOG_STORE_new),
[CTLOG\_STORE\_get0\_log\_by\_id(3)](http://man.he.net/man3/CTLOG_STORE_get0_log_by_id),
[SCT\_new(3)](http://man.he.net/man3/SCT_new),
[SCT\_print(3)](http://man.he.net/man3/SCT_print),
[SCT\_verify(3)](http://man.he.net/man3/SCT_verify),
[SCT\_validate(3)](http://man.he.net/man3/SCT_validate),
[CT\_POLICY\_EVAL\_CTX(3)](http://man.he.net/man3/CT_POLICY_EVAL_CTX),
[SSL\_CTX\_set\_ct\_validation\_callback(3)](http://man.he.net/man3/SSL_CTX_set_ct_validation_callback)

# HISTORY

This library was added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
