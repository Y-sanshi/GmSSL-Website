# NAME

ASYNC\_WAIT\_CTX\_new, ASYNC\_WAIT\_CTX\_free, ASYNC\_WAIT\_CTX\_set\_wait\_fd,
ASYNC\_WAIT\_CTX\_get\_fd, ASYNC\_WAIT\_CTX\_get\_all\_fds,
ASYNC\_WAIT\_CTX\_get\_changed\_fds, ASYNC\_WAIT\_CTX\_clear\_fd - functions to manage
waiting for asynchronous jobs to complete

# SYNOPSIS

    #include <openssl/async.h>

    ASYNC_WAIT_CTX *ASYNC_WAIT_CTX_new(void);
    void ASYNC_WAIT_CTX_free(ASYNC_WAIT_CTX *ctx);
    int ASYNC_WAIT_CTX_set_wait_fd(ASYNC_WAIT_CTX *ctx, const void *key,
                                   OSSL_ASYNC_FD fd,
                                   void *custom_data,
                                   void (*cleanup)(ASYNC_WAIT_CTX *, const void *,
                                                  OSSL_ASYNC_FD, void *));
    int ASYNC_WAIT_CTX_get_fd(ASYNC_WAIT_CTX *ctx, const void *key,
                              OSSL_ASYNC_FD *fd, void **custom_data);
    int ASYNC_WAIT_CTX_get_all_fds(ASYNC_WAIT_CTX *ctx, OSSL_ASYNC_FD *fd,
                                   size_t *numfds);
    int ASYNC_WAIT_CTX_get_changed_fds(ASYNC_WAIT_CTX *ctx, OSSL_ASYNC_FD *addfd,
                                       size_t *numaddfds, OSSL_ASYNC_FD *delfd,
                                       size_t *numdelfds);
    int ASYNC_WAIT_CTX_clear_fd(ASYNC_WAIT_CTX *ctx, const void *key);

# DESCRIPTION

For an overview of how asynchronous operations are implemented in OpenSSL see
[ASYNC\_start\_job(3)](http://man.he.net/man3/ASYNC_start_job). An ASYNC\_WAIT\_CTX object represents an asynchronous
"session", i.e. a related set of crypto operations. For example in SSL terms
this would have a one-to-one correspondence with an SSL connection.

Application code must create an ASYNC\_WAIT\_CTX using the ASYNC\_WAIT\_CTX\_new()
function prior to calling ASYNC\_start\_job() (see [ASYNC\_start\_job(3)](http://man.he.net/man3/ASYNC_start_job)). When
the job is started it is associated with the ASYNC\_WAIT\_CTX for the duration of
that job. An ASYNC\_WAIT\_CTX should only be used for one ASYNC\_JOB at any one
time, but can be reused after an ASYNC\_JOB has finished for a subsequent
ASYNC\_JOB. When the session is complete (e.g. the SSL connection is closed),
application code cleans up with ASYNC\_WAIT\_CTX\_free().

ASYNC\_WAIT\_CTXs can have "wait" file descriptors associated with them. Calling
ASYNC\_WAIT\_CTX\_get\_all\_fds() and passing in a pointer to an ASYNC\_WAIT\_CTX in
the **ctx** parameter will return the wait file descriptors associated with that
job in **\*fd**. The number of file descriptors returned will be stored in
**\*numfds**. It is the caller's responsibility to ensure that sufficient memory
has been allocated in **\*fd** to receive all the file descriptors. Calling
ASYNC\_WAIT\_CTX\_get\_all\_fds() with a NULL **fd** value will return no file
descriptors but will still populate **\*numfds**. Therefore application code is
typically expected to call this function twice: once to get the number of fds,
and then again when sufficient memory has been allocated. If only one
asynchronous engine is being used then normally this call will only ever return
one fd. If multiple asynchronous engines are being used then more could be
returned.

The function ASYNC\_WAIT\_CTX\_fds\_have\_changed() can be used to detect if any fds
have changed since the last call time ASYNC\_start\_job() returned an ASYNC\_PAUSE
result (or since the ASYNC\_WAIT\_CTX was created if no ASYNC\_PAUSE result has
been received). The **numaddfds** and **numdelfds** parameters will be populated
with the number of fds added or deleted respectively. **\*addfd** and **\*delfd**
will be populated with the list of added and deleted fds respectively. Similarly
to ASYNC\_WAIT\_CTX\_get\_all\_fds() either of these can be NULL, but if they are not
NULL then the caller is responsible for ensuring sufficient memory is allocated.

Implementors of async aware code (e.g. engines) are encouraged to return a
stable fd for the lifetime of the ASYNC\_WAIT\_CTX in order to reduce the "churn"
of regularly changing fds - although no guarantees of this are provided to
applications.

Applications can wait for the file descriptor to be ready for "read" using a
system function call such as select or poll (being ready for "read" indicates
that the job should be resumed). If no file descriptor is made available then an
application will have to periodically "poll" the job by attempting to restart it
to see if it is ready to continue.

Async aware code (e.g. engines) can get the current ASYNC\_WAIT\_CTX from the job
via [ASYNC\_get\_wait\_ctx(3)](http://man.he.net/man3/ASYNC_get_wait_ctx) and provide a file descriptor to use for waiting
on by calling ASYNC\_WAIT\_CTX\_set\_wait\_fd(). Typically this would be done by an
engine immediately prior to calling ASYNC\_pause\_job() and not by end user code.
An existing association with a file descriptor can be obtained using
ASYNC\_WAIT\_CTX\_get\_fd() and cleared using ASYNC\_WAIT\_CTX\_clear\_fd(). Both of
these functions requires a **key** value which is unique to the async aware
code.  This could be any unique value but a good candidate might be the
**ENGINE \*** for the engine. The **custom\_data** parameter can be any value, and
will be returned in a subsequent call to ASYNC\_WAIT\_CTX\_get\_fd(). The
ASYNC\_WAIT\_CTX\_set\_wait\_fd() function also expects a pointer to a "cleanup"
routine. This can be NULL but if provided will automatically get called when
the ASYNC\_WAIT\_CTX is freed, and gives the engine the opportunity to close the
fd or any other resources. Note: The "cleanup" routine does not get called if
the fd is cleared directly via a call to ASYNC\_WAIT\_CTX\_clear\_fd().

An example of typical usage might be an async capable engine. User code would
initiate cryptographic operations. The engine would initiate those operations
asynchronously and then call ASYNC\_WAIT\_CTX\_set\_wait\_fd() followed by
ASYNC\_pause\_job() to return control to the user code. The user code can then
perform other tasks or wait for the job to be ready by calling "select" or other
similar function on the wait file descriptor. The engine can signal to the user
code that the job should be resumed by making the wait file descriptor
"readable". Once resumed the engine should clear the wake signal on the wait
file descriptor.

# RETURN VALUES

ASYNC\_WAIT\_CTX\_new() returns a pointer to the newly allocated ASYNC\_WAIT\_CTX or
NULL on error.

ASYNC\_WAIT\_CTX\_set\_wait\_fd, ASYNC\_WAIT\_CTX\_get\_fd, ASYNC\_WAIT\_CTX\_get\_all\_fds,
ASYNC\_WAIT\_CTX\_get\_changed\_fds and ASYNC\_WAIT\_CTX\_clear\_fd all return 1 on
success or 0 on error.

# NOTES

On Windows platforms the openssl/async.h header is dependent on some
of the types customarily made available by including windows.h. The
application developer is likely to require control over when the latter
is included, commonly as one of the first included headers. Therefore
it is defined as an application developer's responsibility to include
windows.h prior to async.h.

# SEE ALSO

[crypto(3)](http://man.he.net/man3/crypto), [ASYNC\_start\_job(3)](http://man.he.net/man3/ASYNC_start_job)

# HISTORY

ASYNC\_WAIT\_CTX\_new, ASYNC\_WAIT\_CTX\_free, ASYNC\_WAIT\_CTX\_set\_wait\_fd,
ASYNC\_WAIT\_CTX\_get\_fd, ASYNC\_WAIT\_CTX\_get\_all\_fds,
ASYNC\_WAIT\_CTX\_get\_changed\_fds, ASYNC\_WAIT\_CTX\_clear\_fd were first added to
OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
