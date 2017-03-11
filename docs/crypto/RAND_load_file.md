# NAME

RAND\_load\_file, RAND\_write\_file, RAND\_file\_name - PRNG seed file

# SYNOPSIS

    #include <openssl/rand.h>

    const char *RAND_file_name(char *buf, size_t num);

    int RAND_load_file(const char *filename, long max_bytes);

    int RAND_write_file(const char *filename);

# DESCRIPTION

RAND\_file\_name() generates a default path for the random seed
file. **buf** points to a buffer of size **num** in which to store the
filename.

On all systems, if the environment variable **RANDFILE** is set, its
value will be used as the seed file name.

Otherwise, the file is called ".rnd", found in platform dependent locations:

- On Windows (in order of preference)

    %HOME%, %USERPROFILE%, %SYSTEMROOT%, C:\\

- On VMS

    SYS$LOGIN:

- On all other systems

    $HOME

If `$HOME` (on non-Windows and non-VMS system) is not set either, or
**num** is too small for the path name, an error occurs.

RAND\_load\_file() reads a number of bytes from file **filename** and
adds them to the PRNG. If **max\_bytes** is non-negative,
up to **max\_bytes** are read;
if **max\_bytes** is -1, the complete file is read.

RAND\_write\_file() writes a number of random bytes (currently 1024) to
file **filename** which can be used to initialize the PRNG by calling
RAND\_load\_file() in a later session.

# RETURN VALUES

RAND\_load\_file() returns the number of bytes read.

RAND\_write\_file() returns the number of bytes written, and -1 if the
bytes written were generated without appropriate seed.

RAND\_file\_name() returns a pointer to **buf** on success, and NULL on
error.

# SEE ALSO

[rand(3)](http://man.he.net/man3/rand), [RAND\_add(3)](http://man.he.net/man3/RAND_add), [RAND\_cleanup(3)](http://man.he.net/man3/RAND_cleanup)

# COPYRIGHT

Copyright 2000-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
