# NAME

OPENSSL\_LH\_stats, OPENSSL\_LH\_node\_stats, OPENSSL\_LH\_node\_usage\_stats,
OPENSSL\_LH\_stats\_bio,
OPENSSL\_LH\_node\_stats\_bio, OPENSSL\_LH\_node\_usage\_stats\_bio - LHASH statistics

# SYNOPSIS

    #include <openssl/lhash.h>

    void OPENSSL_LH_stats(LHASH *table, FILE *out);
    void OPENSSL_LH_node_stats(LHASH *table, FILE *out);
    void OPENSSL_LH_node_usage_stats(LHASH *table, FILE *out);

    void OPENSSL_LH_stats_bio(LHASH *table, BIO *out);
    void OPENSSL_LH_node_stats_bio(LHASH *table, BIO *out);
    void OPENSSL_LH_node_usage_stats_bio(LHASH *table, BIO *out);

# DESCRIPTION

The **LHASH** structure records statistics about most aspects of
accessing the hash table.  This is mostly a legacy of Eric Young
writing this library for the reasons of implementing what looked like
a nice algorithm rather than for a particular software product.

OPENSSL\_LH\_stats() prints out statistics on the size of the hash table, how
many entries are in it, and the number and result of calls to the
routines in this library.

OPENSSL\_LH\_node\_stats() prints the number of entries for each 'bucket' in the
hash table.

OPENSSL\_LH\_node\_usage\_stats() prints out a short summary of the state of the
hash table.  It prints the 'load' and the 'actual load'.  The load is
the average number of data items per 'bucket' in the hash table.  The
'actual load' is the average number of items per 'bucket', but only
for buckets which contain entries.  So the 'actual load' is the
average number of searches that will need to find an item in the hash
table, while the 'load' is the average number that will be done to
record a miss.

OPENSSL\_LH\_stats\_bio(), OPENSSL\_LH\_node\_stats\_bio() and OPENSSL\_LH\_node\_usage\_stats\_bio()
are the same as the above, except that the output goes to a **BIO**.

# RETURN VALUES

These functions do not return values.

# SEE ALSO

[bio(3)](http://man.he.net/man3/bio), [lhash(3)](http://man.he.net/man3/lhash)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
