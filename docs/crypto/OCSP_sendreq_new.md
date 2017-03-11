# NAME

OCSP\_sendreq\_new, OCSP\_sendreq\_nbio, OCSP\_REQ\_CTX\_free,
OCSP\_set\_max\_response\_length, OCSP\_REQ\_CTX\_add1\_header,
OCSP\_REQ\_CTX\_set1\_req, OCSP\_sendreq\_bio - OCSP responder query functions

# SYNOPSIS

    #include <openssl/ocsp.h>

    OCSP_REQ_CTX *OCSP_sendreq_new(BIO *io, const char *path, OCSP_REQUEST *req,
                                   int maxline);

    int OCSP_sendreq_nbio(OCSP_RESPONSE **presp, OCSP_REQ_CTX *rctx);

    void OCSP_REQ_CTX_free(OCSP_REQ_CTX *rctx);

    void OCSP_set_max_response_length(OCSP_REQ_CTX *rctx, unsigned long len);

    int OCSP_REQ_CTX_add1_header(OCSP_REQ_CTX *rctx,
                                 const char *name, const char *value);

    int OCSP_REQ_CTX_set1_req(OCSP_REQ_CTX *rctx, OCSP_REQUEST *req);

    OCSP_RESPONSE *OCSP_sendreq_bio(BIO *io, const char *path, OCSP_REQUEST *req,
                                    int maxline);

# DESCRIPTION

The function OCSP\_sendreq\_new() returns an **OCSP\_CTX** structure using the
responder **io**, the URL path **path**, the OCSP request **req** and with a
response header maximum line length of **maxline**. If **maxline** is zero a
default value of 4k is used. The OCSP request **req** may be set to **NULL**
and provided later if required.

OCSP\_sendreq\_nbio() performs non-blocking I/O on the OCSP request context
**rctx**. When the operation is complete it returns the response in **\*presp**.

OCSP\_REQ\_CTX\_free() frees up the OCSP context **rctx**.

OCSP\_set\_max\_response\_length() sets the maximum response length for **rctx**
to **len**. If the response exceeds this length an error occurs. If not
set a default value of 100k is used.

OCSP\_REQ\_CTX\_add1\_header() adds header **name** with value **value** to the
context **rctx**. It can be called more than once to add multiple headers.
It **MUST** be called before any calls to OCSP\_sendreq\_nbio(). The **req**
parameter in the initial to OCSP\_sendreq\_new() call MUST be set to **NULL** if
additional headers are set.

OCSP\_REQ\_CTX\_set1\_req() sets the OCSP request in **rctx** to **req**. This
function should be called after any calls to OCSP\_REQ\_CTX\_add1\_header().

OCSP\_sendreq\_bio() performs an OCSP request using the responder **io**, the URL
path **path**, the OCSP request **req** and with a response header maximum line
length of **maxline**. If **maxline** is zero a default value of 4k is used.

# RETURN VALUES

OCSP\_sendreq\_new() returns a valid **OCSP\_REQ\_CTX** structure or **NULL** if
an error occurred.

OCSP\_sendreq\_nbio() returns **1** if the operation was completed successfully,
**-1** if the operation should be retried and **0** if an error occurred.

OCSP\_REQ\_CTX\_add1\_header() and OCSP\_REQ\_CTX\_set1\_req() return **1** for success
and **0** for failure.

OCSP\_sendreq\_bio() returns the **OCSP\_RESPONSE** structure sent by the
responder or **NULL** if an error occurred.

OCSP\_REQ\_CTX\_free() and OCSP\_set\_max\_response\_length() do not return values.

# NOTES

These functions only perform a minimal HTTP query to a responder. If an
application wishes to support more advanced features it should use an
alternative more complete HTTP library.

Currently only HTTP POST queries to responders are supported.

The arguments to OCSP\_sendreq\_new() correspond to the components of the URL.
For example if the responder URL is **http://ocsp.com/ocspreq** the BIO
**io** should be connected to host **ocsp.com** on port 80 and **path**
should be set to **"/ocspreq"**

The headers added with OCSP\_REQ\_CTX\_add1\_header() are of the form
"**name**: **value**" or just "**name**" if **value** is **NULL**. So to add
a Host header for **ocsp.com** you would call:

    OCSP_REQ_CTX_add1_header(ctx, "Host", "ocsp.com");

If OCSP\_sendreq\_nbio() indicates an operation should be retried the
corresponding BIO can be examined to determine which operation (read or
write) should be retried and appropriate action taken (for example a select()
call on the underlying socket).

OCSP\_sendreq\_bio() does not support retries and so cannot handle non-blocking
I/O efficiently. It is retained for compatibility and its use in new
applications is not recommended.

# SEE ALSO

[crypto(3)](http://man.he.net/man3/crypto),
[OCSP\_cert\_to\_id(3)](http://man.he.net/man3/OCSP_cert_to_id),
[OCSP\_request\_add1\_nonce(3)](http://man.he.net/man3/OCSP_request_add1_nonce),
[OCSP\_REQUEST\_new(3)](http://man.he.net/man3/OCSP_REQUEST_new),
[OCSP\_response\_find\_status(3)](http://man.he.net/man3/OCSP_response_find_status),
[OCSP\_response\_status(3)](http://man.he.net/man3/OCSP_response_status)

# COPYRIGHT

Copyright 2015-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
