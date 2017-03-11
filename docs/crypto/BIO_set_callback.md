# NAME

BIO\_set\_callback\_ex, BIO\_get\_callback\_ex, BIO\_set\_callback, BIO\_get\_callback,
BIO\_set\_callback\_arg, BIO\_get\_callback\_arg, BIO\_debug\_callback,
BIO\_callback\_fn\_ex, BIO\_callback\_fn
\- BIO callback functions

# SYNOPSIS

    #include <openssl/bio.h>

    typedef long (*BIO_callback_fn_ex)(BIO *b, int oper, const char *argp,
                                       size_t len, int argi,
                                       long argl, int ret, size_t *processed);
    typedef long (*BIO_callback_fn)(BIO *b, int oper, const char *argp, int argi,
                                    long argl, long ret);

    void BIO_set_callback_ex(BIO *b, BIO_callback_fn_ex callback);
    BIO_callback_fn_ex BIO_get_callback_ex(const BIO *b);

    void BIO_set_callback(BIO *b, BIO_callack_fn cb);
    BIO_callack_fn BIO_get_callback(BIO *b);
    void BIO_set_callback_arg(BIO *b, char *arg);
    char *BIO_get_callback_arg(const BIO *b);

    long BIO_debug_callback(BIO *bio, int cmd, const char *argp, int argi,
                            long argl, long ret);

# DESCRIPTION

BIO\_set\_callback\_ex() and BIO\_get\_callback\_ex() set and retrieve the BIO
callback. The callback is called during most high level BIO operations. It can
be used for debugging purposes to trace operations on a BIO or to modify its
operation.

BIO\_set\_callback() and BIO\_get\_callback() set and retrieve the old format BIO
callback. New code should not use these functions, but they are retained for
backwards compatbility. Any callback set via BIO\_set\_callback\_ex() will get
called in preference to any set by BIO\_set\_callback().

BIO\_set\_callback\_arg() and BIO\_get\_callback\_arg() are macros which can be
used to set and retrieve an argument for use in the callback.

BIO\_debug\_callback() is a standard debugging callback which prints
out information relating to each BIO operation. If the callback
argument is set it is interpreted as a BIO to send the information
to, otherwise stderr is used.

BIO\_callback\_fn\_ex() is the type of the callback function and BIO\_callback\_fn()
is the type of the old format callback function. The meaning of each argument
is described below:

- **b**

    The BIO the callback is attached to is passed in **b**.

- **oper**

    **oper** is set to the operation being performed. For some operations
    the callback is called twice, once before and once after the actual
    operation, the latter case has **oper** or'ed with BIO\_CB\_RETURN.

- **len**

    The length of the data requested to be read or written. This is only useful if
    **oper** is BIO\_CB\_READ, BIO\_CB\_WRITE or BIO\_CB\_GETS.

- **argp** **argi** **argl**

    The meaning of the arguments **argp**, **argi** and **argl** depends on
    the value of **oper**, that is the operation being performed.

- **processed**

    **processed** is a pointer to a location which will be updated with the amount of
    data that was actually read or written. Only used for BIO\_CB\_READ, BIO\_CB\_WRITE,
    BIO\_CB\_GETS and BIO\_CB\_PUTS.

- **ret**

    **ret** is the return value that would be returned to the
    application if no callback were present. The actual value returned
    is the return value of the callback itself. In the case of callbacks
    called before the actual BIO operation 1 is placed in **ret**, if
    the return value is not positive it will be immediately returned to
    the application and the BIO operation will not be performed.

The callback should normally simply return **ret** when it has
finished processing, unless it specifically wishes to modify the
value returned to the application.

# CALLBACK OPERATIONS

In the notes below, **callback** defers to the actual callback
function that is called.

- **BIO\_free(b)**

        callback_ex(b, BIO_CB_FREE, NULL, 0, 0, 0L, 1L, NULL)

    or

        callback(b, BIO_CB_FREE, NULL, 0L, 0L, 1L)

    is called before the free operation.

- **BIO\_read\_ex(b, data, dlen, readbytes)**

        callback_ex(b, BIO_CB_READ, data, dlen, 0, 0L, 1L, readbytes)

    or

        callback(b, BIO_CB_READ, data, dlen, 0L, 1L)

    is called before the read and

        callback_ex(b, BIO_CB_READ | BIO_CB_RETURN, data, dlen, 0, 0L, retvalue, readbytes)

    or

        callback(b, BIO_CB_READ|BIO_CB_RETURN, data, dlen, 0L, retvalue)

    after.

- **BIO\_write(b, data, dlen, written)**

        callback_ex(b, BIO_CB_WRITE, data, dlen, 0, 0L, 1L, written)

    or

        callback(b, BIO_CB_WRITE, datat, dlen, 0L, 1L)

    is called before the write and

        callback_ex(b, BIO_CB_WRITE | BIO_CB_RETURN, data, dlen, 0, 0L, retvalue, written)

    or

        callback(b, BIO_CB_WRITE|BIO_CB_RETURN, data, dlen, 0L, retvalue)

    after.

- **BIO\_gets(b, buf, size)**

        callback_ex(b, BIO_CB_GETS, buf, size, 0, 0L, 1, NULL, NULL)

    or

        callback(b, BIO_CB_GETS, buf, size, 0L, 1L)

    is called before the operation and

        callback_ex(b, BIO_CB_GETS | BIO_CB_RETURN, buf, size, 0, 0L, retvalue, readbytes)

    or

        callback(b, BIO_CB_GETS|BIO_CB_RETURN, buf, size, 0L, retvalue)

    after.

- **BIO\_puts(b, buf)**

        callback_ex(b, BIO_CB_PUTS, buf, 0, 0, 0L, 1L, NULL);

    or

        callback(b, BIO_CB_PUTS, buf, 0, 0L, 1L)

    is called before the operation and

        callback_ex(b, BIO_CB_PUTS | BIO_CB_RETURN, buf, 0, 0, 0L, retvalue, written)

    or

        callback(b, BIO_CB_WRITE|BIO_CB_RETURN, buf, 0, 0L, retvalue)

    after.

- **BIO\_ctrl(BIO \*b, int cmd, long larg, void \*parg)**

        callback_ex(b, BIO_CB_CTRL, parg, 0, cmd, larg, 1L, NULL)

    or

        callback(b, BIO_CB_CTRL, parg, cmd, larg, 1L)

    is called before the call and

        callback_ex(b, BIO_CB_CTRL | BIO_CB_RETURN, parg, 0, cmd, larg, ret, NULL)

    or

        callback(b, BIO_CB_CTRL|BIO_CB_RETURN, parg, cmd, larg, ret)

    after.

# EXAMPLE

The BIO\_debug\_callback() function is a good example, its source is
in crypto/bio/bio\_cb.c

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
