# NAME

CTLOG\_STORE\_get0\_log\_by\_id -
Get a Certificate Transparency log from a CTLOG\_STORE

# SYNOPSIS

    #include <openssl/ct.h>

    const CTLOG *CTLOG_STORE_get0_log_by_id(const CTLOG_STORE *store,
                                            const uint8_t *log_id,
                                            size_t log_id_len);

# DESCRIPTION

A Signed Certificate Timestamp (SCT) identifies the Certificate Transparency
(CT) log that issued it using the log's LogID (see RFC 6962, Section 3.2).
Therefore, it is useful to be able to look up more information about a log
(e.g. its public key) using this LogID.

CTLOG\_STORE\_get0\_log\_by\_id() provides a way to do this. It will find a CTLOG
in a CTLOG\_STORE that has a given LogID.

# RETURN VALUES

**CTLOG\_STORE\_get0\_log\_by\_id** returns a CTLOG with the given LogID, if it
exists in the given CTLOG\_STORE, otherwise it returns NULL.

# SEE ALSO

[ct(3)](http://man.he.net/man3/ct),
[CTLOG\_STORE\_new(3)](http://man.he.net/man3/CTLOG_STORE_new)

# HISTORY

This function was added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
