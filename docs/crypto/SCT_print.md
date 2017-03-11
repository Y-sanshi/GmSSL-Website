# NAME

SCT\_print, SCT\_LIST\_print, SCT\_validation\_status\_string -
Prints Signed Certificate Timestamps in a human-readable way

# SYNOPSIS

    #include <openssl/ct.h>

    void SCT_print(const SCT *sct, BIO *out, int indent, const CTLOG_STORE *logs);
    void SCT_LIST_print(const STACK_OF(SCT) *sct_list, BIO *out, int indent,
                        const char *separator, const CTLOG_STORE *logs);
    const char *SCT_validation_status_string(const SCT *sct);

# DESCRIPTION

SCT\_print() prints a single Signed Certificate Timestamp (SCT) to a [bio](https://metacpan.org/pod/bio) in
a human-readable format. SCT\_LIST\_print() prints an entire list of SCTs in a
similar way. A separator can be specified to delimit each SCT in the output.

The output can be indented by a specified number of spaces. If a **CTLOG\_STORE**
is provided, it will be used to print the description of the CT log that issued
each SCT (if that log is in the CTLOG\_STORE). Alternatively, NULL can be passed
as the CTLOG\_STORE parameter to disable this feature.

SCT\_validation\_status\_string() will return the validation status of an SCT as
a human-readable string. Call SCT\_validate() or SCT\_LIST\_validate()
beforehand in order to set the validation status of an SCT first.

# SEE ALSO

[ct(3)](http://man.he.net/man3/ct),
[bio(3)](http://man.he.net/man3/bio),
[CTLOG\_STORE\_new(3)](http://man.he.net/man3/CTLOG_STORE_new),
[SCT\_validate(3)](http://man.he.net/man3/SCT_validate)

# HISTORY

These functions were added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
