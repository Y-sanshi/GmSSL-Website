# NAME

OPENSSL\_init\_new, OPENSSL\_INIT\_set\_config\_appname, OPENSSL\_INIT\_free,
OPENSSL\_init\_crypto, OPENSSL\_cleanup,
OPENSSL\_atexit, OPENSSL\_thread\_stop - OpenSSL
initialisation and deinitialisation functions

# SYNOPSIS

    #include <openssl/crypto.h>

    void OPENSSL_cleanup(void);
    int OPENSSL_init_crypto(uint64_t opts, const OPENSSL_INIT_SETTINGS *settings);
    int OPENSSL_atexit(void (*handler)(void));
    void OPENSSL_thread_stop(void);

    OPENSSL_INIT_SETTINGS *OPENSSL_init_new(void);
    int OPENSSL_INIT_set_config_appname(OPENSSL_INIT_SETTINGS *init,
                                        const char* name);
    void OPENSSL_INIT_free(OPENSSL_INIT_SETTINGS *init);

# DESCRIPTION

During normal operation OpenSSL (libcrypto) will allocate various resources at
start up that must, subsequently, be freed on close down of the library.
Additionally some resources are allocated on a per thread basis (if the
application is multi-threaded), and these resources must be freed prior to the
thread closing.

As of version 1.1.0 OpenSSL will automatically allocate all resources that it
needs so no explicit initialisation is required. Similarly it will also
automatically deinitialise as required.

However, there way be situations when explicit initialisation is desirable or
needed, for example when some non-default initialisation is required. The
function OPENSSL\_init\_crypto() can be used for this purpose for
libcrypto (see also [OPENSSL\_init\_ssl(3)](http://man.he.net/man3/OPENSSL_init_ssl) for the libssl
equivalent).

Numerous internal OpenSSL functions call OPENSSL\_init\_crypto().
Therefore, in order to perform non-default initialisation,
OPENSSL\_init\_crypto() MUST be called by application code prior to
any other OpenSSL function calls.

The **opts** parameter specifies which aspects of libcrypto should be
initialised. Valid options are:

- OPENSSL\_INIT\_NO\_LOAD\_CRYPTO\_STRINGS

    Suppress automatic loading of the libcrypto error strings. This option is
    not a default option. Once selected subsequent calls to
    OPENSSL\_init\_crypto() with the option
    **OPENSSL\_INIT\_LOAD\_CRYPTO\_STRINGS** will be ignored.

- OPENSSL\_INIT\_LOAD\_CRYPTO\_STRINGS

    Automatic loading of the libcrypto error strings. With this option the
    library will automatically load the libcrypto error strings.
    This option is a default option. Once selected subsequent calls to
    OPENSSL\_init\_crypto() with the option
    **OPENSSL\_INIT\_NO\_LOAD\_CRYPTO\_STRINGS** will be ignored.

- OPENSSL\_INIT\_ADD\_ALL\_CIPHERS

    With this option the library will automatically load and make available all
    libcrypto ciphers. This option is a default option. Once selected subsequent
    calls to OPENSSL\_init\_crypto() with the option
    **OPENSSL\_INIT\_NO\_ADD\_ALL\_CIPHERS** will be ignored.

- OPENSSL\_INIT\_ADD\_ALL\_DIGESTS

    With this option the library will automatically load and make available all
    libcrypto digests. This option is a default option. Once selected subsequent
    calls to OPENSSL\_init\_crypto() with the option
    **OPENSSL\_INIT\_NO\_ADD\_ALL\_CIPHERS** will be ignored.

- OPENSSL\_INIT\_NO\_ADD\_ALL\_CIPHERS

    With this option the library will suppress automatic loading of libcrypto
    ciphers. This option is not a default option. Once selected subsequent
    calls to OPENSSL\_init\_crypto() with the option
    **OPENSSL\_INIT\_ADD\_ALL\_CIPHERS** will be ignored.

- OPENSSL\_INIT\_NO\_ADD\_ALL\_DIGESTS

    With this option the library will suppress automatic loading of libcrypto
    digests. This option is not a default option. Once selected subsequent
    calls to OPENSSL\_init\_crypto() with the option
    **OPENSSL\_INIT\_ADD\_ALL\_DIGESTS** will be ignored.

- OPENSSL\_INIT\_LOAD\_CONFIG

    With this option an OpenSSL configuration file will be automatically loaded and
    used by calling OPENSSL\_config(). This is not a default option.
    See the description of OPENSSL\_init\_new(), below.

- OPENSSL\_INIT\_NO\_LOAD\_CONFIG

    With this option the loading of OpenSSL configuration files will be suppressed.
    It is the equivalent of calling OPENSSL\_no\_config(). This is not a default
    option.

- OPENSSL\_INIT\_ASYNC

    With this option the library with automatically initialise the libcrypto async
    sub-library (see [ASYNC\_start\_job(3)](http://man.he.net/man3/ASYNC_start_job)). This is a default option.

- OPENSSL\_INIT\_ENGINE\_RDRAND

    With this option the library will automatically load and initialise the
    RDRAND engine (if available). This not a default option.

- OPENSSL\_INIT\_ENGINE\_DYNAMIC

    With this option the library will automatically load and initialise the
    dynamic engine. This not a default option.

- OPENSSL\_INIT\_ENGINE\_OPENSSL

    With this option the library will automatically load and initialise the
    openssl engine. This not a default option.

- OPENSSL\_INIT\_ENGINE\_CRYPTODEV

    With this option the library will automatically load and initialise the
    cryptodev engine (if available). This not a default option.

- OPENSSL\_INIT\_ENGINE\_CAPI

    With this option the library will automatically load and initialise the
    CAPI engine (if available). This not a default option.

- OPENSSL\_INIT\_ENGINE\_PADLOCK

    With this option the library will automatically load and initialise the
    padlock engine (if available). This not a default option.

- OPENSSL\_INIT\_ENGINE\_DASYNC

    With this option the library will automatically load and initialise the
    DASYNC engine. This not a default option.

- OPENSSL\_INIT\_ENGINE\_ALL\_BUILTIN

    With this option the library will automatically load and initialise all the
    built in engines listed above with the exception of the openssl and dasync
    engines. This not a default option.

Multiple options may be combined together in a single call to
OPENSSL\_init\_crypto(). For example:

    OPENSSL_init_crypto(OPENSSL_INIT_NO_ADD_ALL_CIPHERS
                        | OPENSSL_INIT_NO_ADD_ALL_DIGESTS, NULL);

The OPENSSL\_cleanup() function deinitialises OpenSSL (both libcrypto
and libssl). All resources allocated by OpenSSL are freed. Typically there
should be no need to call this function directly as it is initiated
automatically on application exit. This is done via the standard C library
[atexit(3)](http://man.he.net/man3/atexit) function. In the event that the application will close in a manner
that will not call the registered atexit() handlers then the application should
call OPENSSL\_cleanup() directly. Developers of libraries using OpenSSL
are discouraged from calling this function and should instead, typically, rely
on auto-deinitialisation. This is to avoid error conditions where both an
application and a library it depends on both use OpenSSL, and the library
deinitialises it before the application has finished using it.

Once OPENSSL\_cleanup() has been called the library cannot be reinitialised.
Attempts to call OPENSSL\_init\_crypto() will fail and an ERR\_R\_INIT\_FAIL error
will be added to the error stack. Note that because initialisation has failed
OpenSSL error strings will not be available, only an error code. This code can
be put through the openssl errstr command line application to produce a human
readable error (see [errstr(1)](http://man.he.net/man1/errstr)).

The OPENSSL\_atexit() function enables the registration of a
function to be called during OPENSSL\_cleanup(). Stop handlers are
called after deinitialisation of resources local to a thread, but before other
process wide resources are freed. In the event that multiple stop handlers are
registered, no guarantees are made about the order of execution.

The OPENSSL\_thread\_stop() function deallocates resources associated
with the current thread. Typically this function will be called automatically by
the library when the thread exits. This should only be called directly if
resources should be freed at an earlier time, or under the circumstances
described in the NOTES section below.

The **OPENSSL\_INIT\_LOAD\_CONFIG** flag will load a default configuration
file.  To specify a different file, an **OPENSSL\_INIT\_SETTINGS** must
be created and used. The routines
OPENSSL\_init\_new() and OPENSSL\_INIT\_set\_config\_appname() can be used to
allocate the object and set the application name, and then the
object can be released with OPENSSL\_INIT\_free() when done.

# NOTES

Resources local to a thread are deallocated automatically when the thread exits
(e.g. in a pthreads environment, when pthread\_exit() is called). On Windows
platforms this is done in response to a DLL\_THREAD\_DETACH message being sent to
the libcrypto32.dll entry point. Some windows functions may cause threads to exit
without sending this message (for example ExitProcess()). If the application
uses such functions, then the application must free up OpenSSL resources
directly via a call to OPENSSL\_thread\_stop() on each thread. Similarly this
message will also not be sent if OpenSSL is linked statically, and therefore
applications using static linking should also call OPENSSL\_thread\_stop() on each
thread. Additionally if OpenSSL is loaded dynamically via LoadLibrary() and the
threads are not destroyed until after FreeLibrary() is called then each thread
should call OPENSSL\_thread\_stop() prior to the FreeLibrary() call.

On Linux/Unix where OpenSSL has been loaded via dlopen() and the application is
multi-threaded and if dlclose() is subsequently called prior to the threads
being destroyed then OpenSSL will not be able to deallocate resources associated
with those threads. The application should either call OPENSSL\_thread\_stop() on
each thread prior to the dlclose() call, or alternatively the original dlopen()
call should use the RTLD\_NODELETE flag (where available on the platform).

# RETURN VALUES

The functions OPENSSL\_init\_crypto, OPENSSL\_atexit() and
OPENSSL\_INIT\_set\_config\_appname() return 1 on success or 0 on error.

# SEE ALSO

[OPENSSL\_init\_ssl(3)](http://man.he.net/man3/OPENSSL_init_ssl)

# HISTORY

The OPENSSL\_init\_crypto(), OPENSSL\_cleanup(), OPENSSL\_atexit(),
OPENSSL\_thread\_stop(), OPENSSL\_init\_new(), OPENSSL\_INIT\_set\_config\_appname()
and OPENSSL\_INIT\_free() functions were added in OpenSSL 1.1.0.

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
