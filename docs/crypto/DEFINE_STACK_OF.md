# NAME

DEFINE\_STACK\_OF, DEFINE\_STACK\_OF\_CONST, DEFINE\_SPECIAL\_STACK\_OF,
DEFINE\_SPECIAL\_STACK\_OF\_CONST,
OPENSSL\_sk\_deep\_copy, OPENSSL\_sk\_delete, OPENSSL\_sk\_delete\_ptr,
OPENSSL\_sk\_dup, OPENSSL\_sk\_find, OPENSSL\_sk\_find\_ex, OPENSSL\_sk\_free,
OPENSSL\_sk\_insert, OPENSSL\_sk\_is\_sorted, OPENSSL\_sk\_new, OPENSSL\_sk\_new\_null,
OPENSSL\_sk\_num, OPENSSL\_sk\_pop, OPENSSL\_sk\_pop\_free, OPENSSL\_sk\_push,
OPENSSL\_sk\_set, OPENSSL\_sk\_set\_cmp\_func, OPENSSL\_sk\_shift, OPENSSL\_sk\_sort,
OPENSSL\_sk\_unshift, OPENSSL\_sk\_value, OPENSSL\_sk\_zero,
sk\_TYPE\_num, sk\_TYPE\_value, sk\_TYPE\_new, sk\_TYPE\_new\_null, sk\_TYPE\_free,
sk\_TYPE\_zero, sk\_TYPE\_delete, sk\_TYPE\_delete\_ptr, sk\_TYPE\_push,
sk\_TYPE\_unshift, sk\_TYPE\_pop, sk\_TYPE\_shift, sk\_TYPE\_pop\_free,
sk\_TYPE\_insert, sk\_TYPE\_set, sk\_TYPE\_find, sk\_TYPE\_find\_ex, sk\_TYPE\_sort,
sk\_TYPE\_is\_sorted, sk\_TYPE\_dup, sk\_TYPE\_deep\_copy, sk\_TYPE\_set\_cmp\_func -
stack container

# SYNOPSIS

    #include <openssl/safestack.h>

    STACK_OF(TYPE)
    DEFINE_STACK_OF(TYPE)
    DEFINE_STACK_OF_CONST(TYPE)
    DEFINE_SPECIAL_STACK_OF(FUNCTYPE, TYPE)
    DEFINE_SPECIAL_STACK_OF_CONST(FUNCTYPE, TYPE)

    typedef int (*sk_TYPE_compfunc)(const TYPE *const *a, const TYPE *const *b);
    typedef TYPE * (*sk_TYPE_copyfunc)(const TYPE *a);
    typedef void (*sk_TYPE_freefunc)(TYPE *a);

    int sk_TYPE_num(const STACK_OF(TYPE) *sk);
    TYPE *sk_TYPE_value(const STACK_OF(TYPE) *sk, int idx);
    STACK_OF(TYPE) *sk_TYPE_new(sk_TYPE_compfunc compare);
    STACK_OF(TYPE) *sk_TYPE_new_null(void);
    void sk_TYPE_free(const STACK_OF(TYPE) *sk);
    void sk_TYPE_zero(const STACK_OF(TYPE) *sk);
    TYPE *sk_TYPE_delete(STACK_OF(TYPE) *sk, int i);
    TYPE *sk_TYPE_delete_ptr(STACK_OF(TYPE) *sk, TYPE *ptr);
    int sk_TYPE_push(STACK_OF(TYPE) *sk, const TYPE *ptr);
    int sk_TYPE_unshift(STACK_OF(TYPE) *sk, const TYPE *ptr);
    TYPE *sk_TYPE_pop(STACK_OF(TYPE) *sk);
    TYPE *sk_TYPE_shift(STACK_OF(TYPE) *sk);
    void sk_TYPE_pop_free(STACK_OF(TYPE) *sk, sk_TYPE_freefunc freefunc);
    int sk_TYPE_insert(STACK_OF(TYPE) *sk, TYPE *ptr, int idx);
    TYPE *sk_TYPE_set(STACK_OF(TYPE) *sk, int idx, const TYPE *ptr);
    int sk_TYPE_find(STACK_OF(TYPE) *sk, TYPE *ptr);
    int sk_TYPE_find_ex(STACK_OF(TYPE) *sk, TYPE *ptr);
    void sk_TYPE_sort(const STACK_OF(TYPE) *sk);
    int sk_TYPE_is_sorted(const STACK_OF(TYPE) *sk);
    STACK_OF(TYPE) *sk_TYPE_dup(const STACK_OF(TYPE) *sk);
    STACK_OF(TYPE) *sk_TYPE_deep_copy(const STACK_OF(TYPE) *sk,
                                      sk_TYPE_copyfunc copyfunc,
                                      sk_TYPE_freefunc freefunc);
    sk_TYPE_compfunc (*sk_TYPE_set_cmp_func(STACK_OF(TYPE) *sk, sk_TYPE_compfunc compare);

# DESCRIPTION

Applications can create and use their own stacks by placing any of the macros
described below in a header file. These macros define typesafe inline
functions that wrap around the utility **OPENSSL\_sk\_** API.
In the description here, _TYPE_ is used
as a placeholder for any of the OpenSSL datatypes, such as _X509_.

STACK\_OF() returns the name for a stack of the specified **TYPE**.
DEFINE\_STACK\_OF() creates set of functions for a stack of **TYPE**. This
will mean that type **TYPE** is stored in each stack, the type is referenced by
STACK\_OF(TYPE) and each function name begins with _sk\_TYPE\__. For example:

    TYPE *sk_TYPE_value(STACK_OF(TYPE) *sk, int idx);

DEFINE\_STACK\_OF\_CONST() is identical to DEFINE\_STACK\_OF() except
each element is constant. For example:

    const TYPE *sk_TYPE_value(STACK_OF(TYPE) *sk, int idx);

DEFINE\_SPECIAL\_STACK\_OF() defines a stack of **TYPE** but
each function uses **FUNCNAME** in the function name. For example:

    TYPE *sk_FUNCNAME_value(STACK_OF(TYPE) *sk, int idx);

DEFINE\_SPECIAL\_STACK\_OF\_CONST() is similar except that each element is
constant:

    const TYPE *sk_FUNCNAME_value(STACK_OF(TYPE) *sk, int idx);

sk\_TYPE\_num() returns the number of elements in **sk** or -1 if **sk** is
**NULL**.

sk\_TYPE\_value() returns element **idx** in **sk**, where **idx** starts at
zero. If **idx** is out of range then **NULL** is returned.

sk\_TYPE\_new() allocates a new empty stack using comparison function **compar**.
If **compar** is **NULL** then no comparison function is used.

sk\_TYPE\_new\_null() allocates a new empty stack with no comparison function.

sk\_TYPE\_set\_cmp\_func() sets the comparison function of **sk** to **compar**.
The previous comparison function is returned or **NULL** if there was
no previous comparison function.

sk\_TYPE\_free() frees up the **sk** structure. It does **not** free up any
elements of **sk**. After this call **sk** is no longer valid.

sk\_TYPE\_zero() sets the number of elements in **sk** to zero. It does not free
**sk** so after this call **sk** is still valid.

sk\_TYPE\_pop\_free() frees up all elements of **sk** and **sk** itself. The
free function freefunc() is called on each element to free it.

sk\_TYPE\_delete() deletes element **i** from **sk**. It returns the deleted
element or **NULL** if **i** is out of range.

sk\_TYPE\_delete\_ptr() deletes element matching **ptr** from **sk**. It returns
the deleted element or **NULL** if no element matching **ptr** was found.

sk\_TYPE\_insert() inserts **ptr** into **sk** at position **idx**. Any existing
elements at or after **idx** are moved downwards. If **idx** is out of range
the new element is appended to **sk**. sk\_TYPE\_insert() either returns the
number of elements in **sk** after the new element is inserted or zero if
an error (such as memory allocation failure) occurred.

sk\_TYPE\_push() appends **ptr** to **sk** it is equivalent to:

    sk_TYPE_insert(sk, ptr, -1);

sk\_TYPE\_unshift() inserts **ptr** at the start of **sk** it is equivalent to:

    sk_TYPE_insert(sk, ptr, 0);

sk\_TYPE\_pop() returns and removes the last element from **sk**.

sk\_TYPE\_shift() returns and removes the first element from **sk**.

sk\_TYPE\_set() sets element **idx** of **sk** to **ptr** replacing the current
element. The new element value is returned or **NULL** if an error occurred:
this will only happen if **sk** is **NULL** or **idx** is out of range.

sk\_TYPE\_find() and sk\_TYPE\_find\_ex() search **sk** using the supplied
comparison function for an element matching **ptr**. sk\_TYPE\_find() returns
the index of the first matching element or **-1** if there is no match.
sk\_TYPE\_find\_ex() returns a matching element or the nearest element that
does not match **ptr**. Note: if a comparison function is set then  **sk** is
sorted before the search which may change its order. If no comparison
function is set then a linear search is made for a pointer matching **ptr**
and the stack is not reordered.

sk\_TYPE\_sort() sorts **sk** using the supplied comparison function.

sk\_TYPE\_is\_sorted() returns **1** if **sk** is sorted and **0** otherwise.

sk\_TYPE\_dup() returns a copy of **sk**. Note the pointers in the copy
are identical to the original.

sk\_TYPE\_deep\_copy() returns a new stack where each element has been copied.
Copying is performed by the supplied copyfunc() and freeing by freefunc(). The
function freefunc() is only called if an error occurs.

# NOTES

Care should be taken when accessing stacks in multi-threaded environments.
Any operation which increases the size of a stack such as sk\_TYPE\_insert() or
sk\_push() can "grow" the size of an internal array and cause race conditions
if the same stack is accessed in a different thread. Operations such as
sk\_find() and sk\_sort() can also reorder the stack.

Any comparison function supplied should use a metric suitable
for use in a binary search operation. That is it should return zero, a
positive or negative value if **a** is equal to, greater than
or less than **b** respectively.

Care should be taken when checking the return values of the functions
sk\_TYPE\_find() and sk\_TYPE\_find\_ex(). They return an index to the
matching element. In particular **0** indicates a matching first element.
A failed search is indicated by a **-1** return value.

STACK\_OF(), DEFINE\_STACK\_OF(), DEFINE\_STACK\_OF\_CONST(), and
DEFINE\_SPECIAL\_STACK\_OF() are implemented as macros.

# RETURN VALUES

sk\_TYPE\_num() returns the number of elements in the stack or **-1** if the
passed stack is **NULL**.

sk\_TYPE\_value() returns a pointer to a stack element or **NULL** if the
index is out of range.

sk\_TYPE\_new() and sk\_TYPE\_new\_null() return an empty stack or **NULL** if
an error occurs.

sk\_TYPE\_set\_cmp\_func() returns the old comparison function or **NULL** if
there was no old comparison function.

sk\_TYPE\_free(), sk\_TYPE\_zero(), sk\_TYPE\_pop\_free() and sk\_TYPE\_sort() do
not return values.

sk\_TYPE\_pop(), sk\_TYPE\_shift(), sk\_TYPE\_delete() and sk\_TYPE\_delete\_ptr()
return a pointer to the deleted element or **NULL** on error.

sk\_TYPE\_insert(), sk\_TYPE\_push() and sk\_TYPE\_unshift() return the total
number of elements in the stack and 0 if an error occurred.

sk\_TYPE\_set() returns a pointer to the replacement element or **NULL** on
error.

sk\_TYPE\_find() and sk\_TYPE\_find\_ex() return an index to the found element
or **-1** on error.

sk\_TYPE\_is\_sorted() returns **1** if the stack is sorted and **0** if it is
not.

sk\_TYPE\_dup() and sk\_TYPE\_deep\_copy() return a pointer to the copy of the
stack.

# HISTORY

Before OpenSSL 1.1.0, this was implemented via macros and not inline functions
and was not a public API.

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
