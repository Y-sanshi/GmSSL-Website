# NAME

X509\_STORE\_new, X509\_STORE\_up\_ref, X509\_STORE\_free, X509\_STORE\_lock,
X509\_STORE\_unlock - X509\_STORE allocation, freeing and locking functions

# SYNOPSIS

    #include <openssl/x509_vfy.h>

    X509_STORE *X509_STORE_new(void);
    void X509_STORE_free(X509_STORE *v);
    int X509_STORE_lock(X509_STORE *v);
    int X509_STORE_unlock(X509_STORE *v);
    int X509_STORE_up_ref(X509_STORE *v);

# DESCRIPTION

The X509\_STORE\_new() function returns a new X509\_STORE.

X509\_STORE\_up\_ref() increments the reference count associated with the
X509\_STORE object.

X509\_STORE\_lock() locks the store from modification by other threads,
X509\_STORE\_unlock() locks it.

X509\_STORE\_free() frees up a single X509\_STORE object.

# RETURN VALUES

X509\_STORE\_new() returns a newly created X509\_STORE or NULL if the call fails.

X509\_STORE\_up\_ref(), X509\_STORE\_lock() and X509\_STORE\_unlock() return
1 for success and 0 for failure.

X509\_STORE\_free() does not return values.

# SEE ALSO

[X509\_STORE\_set\_verify\_cb\_func(3)](http://man.he.net/man3/X509_STORE_set_verify_cb_func)
[X509\_STORE\_get0\_param(3)](http://man.he.net/man3/X509_STORE_get0_param)

# HISTORY

The X509\_STORE\_up\_ref(), X509\_STORE\_lock() and X509\_STORE\_unlock()
functions were added in OpenSSL 1.1.0

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
