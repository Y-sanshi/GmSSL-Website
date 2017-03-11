# NAME

CONF\_modules\_load\_file, CONF\_modules\_load - OpenSSL configuration functions

# SYNOPSIS

    #include <openssl/conf.h>

    int CONF_modules_load_file(const char *filename, const char *appname,
                                           unsigned long flags);
    int CONF_modules_load(const CONF *cnf, const char *appname,
                                  unsigned long flags);

# DESCRIPTION

The function CONF\_modules\_load\_file() configures OpenSSL using file
**filename** and application name **appname**. If **filename** is NULL
the standard OpenSSL configuration file is used. If **appname** is
NULL the standard OpenSSL application name **openssl\_conf** is used.
The behaviour can be customized using **flags**.

CONF\_modules\_load() is identical to CONF\_modules\_load\_file() except it
reads configuration information from **cnf**.

# NOTES

The following **flags** are currently recognized:

**CONF\_MFLAGS\_IGNORE\_ERRORS** if set errors returned by individual
configuration modules are ignored. If not set the first module error is
considered fatal and no further modules are loaded.

Normally any modules errors will add error information to the error queue. If
**CONF\_MFLAGS\_SILENT** is set no error information is added.

If **CONF\_MFLAGS\_NO\_DSO** is set configuration module loading from DSOs is
disabled.

**CONF\_MFLAGS\_IGNORE\_MISSING\_FILE** if set will make CONF\_load\_modules\_file()
ignore missing configuration files. Normally a missing configuration file
return an error.

**CONF\_MFLAGS\_DEFAULT\_SECTION** if set and **appname** is not NULL will use the
default section pointed to by **openssl\_conf** if **appname** does not exist.

By using CONF\_modules\_load\_file() with appropriate flags an application can
customise application configuration to best suit its needs. In some cases the
use of a configuration file is optional and its absence is not an error: in
this case **CONF\_MFLAGS\_IGNORE\_MISSING\_FILE** would be set.

Errors during configuration may also be handled differently by different
applications. For example in some cases an error may simply print out a warning
message and the application continue. In other cases an application might
consider a configuration file error as fatal and exit immediately.

Applications can use the CONF\_modules\_load() function if they wish to load a
configuration file themselves and have finer control over how errors are
treated.

# EXAMPLES

Load a configuration file and print out any errors and exit (missing file
considered fatal):

    if (CONF_modules_load_file(NULL, NULL, 0) <= 0) {
       fprintf(stderr, "FATAL: error loading configuration file\n");
       ERR_print_errors_fp(stderr);
       exit(1);
    }

Load default configuration file using the section indicated by "myapp",
tolerate missing files, but exit on other errors:

    if (CONF_modules_load_file(NULL, "myapp",
                               CONF_MFLAGS_IGNORE_MISSING_FILE) <= 0) {
       fprintf(stderr, "FATAL: error loading configuration file\n");
       ERR_print_errors_fp(stderr);
       exit(1);
    }

Load custom configuration file and section, only print warnings on error,
missing configuration file ignored:

    if (CONF_modules_load_file("/something/app.cnf", "myapp",
                               CONF_MFLAGS_IGNORE_MISSING_FILE) <= 0) {
       fprintf(stderr, "WARNING: error loading configuration file\n");
       ERR_print_errors_fp(stderr);
    }

Load and parse configuration file manually, custom error handling:

    FILE *fp;
    CONF *cnf = NULL;
    long eline;
    fp = fopen("/somepath/app.cnf", "r");
    if (fp == NULL) {
       fprintf(stderr, "Error opening configuration file\n");
       /* Other missing configuration file behaviour */
    } else {
       cnf = NCONF_new(NULL);
       if (NCONF_load_fp(cnf, fp, &eline) == 0) {
           fprintf(stderr, "Error on line %ld of configuration file\n", eline);
           ERR_print_errors_fp(stderr);
           /* Other malformed configuration file behaviour */
       } else if (CONF_modules_load(cnf, "appname", 0) <= 0) {
         fprintf(stderr, "Error configuring application\n");
         ERR_print_errors_fp(stderr);
         /* Other configuration error behaviour */
       }
       fclose(fp);
       NCONF_free(cnf);
     }

# RETURN VALUES

These functions return 1 for success and a zero or negative value for
failure. If module errors are not ignored the return code will reflect the
return value of the failing module (this will always be zero or negative).

# SEE ALSO

[conf(5)](http://man.he.net/man5/conf), [OPENSSL\_config(3)](http://man.he.net/man3/OPENSSL_config),
[CONF\_free(3)](http://man.he.net/man3/CONF_free), [err(3)](http://man.he.net/man3/err)

# COPYRIGHT

Copyright 2004-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
