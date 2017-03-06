# NAME

engine - load and query engines

# SYNOPSIS

**gmssl engine**
\[ _engine..._ \]
\[**-v**\]
\[**-vv**\]
\[**-vvv**\]
\[**-vvv**\]
\[**-vvv**\]
\[**-c**\]
\[**-t**\]
\[**-tt**\]
\[**-pre** _command_\]
\[**-post** _command_\]
\[ _engine..._ \]

# DESCRIPTION

The **engine** command is used to query the status and capabilities
of the specified **engine**'s.
Engines may be specified before and after all other command-line flags.
Only those specified are queried.

# OPTIONS

- **-v** **-vv** **-vvv** **-vvvv**

    Provides information about each specified engine. The first flag lists
    all the possible run-time control commands; the second adds a
    description of each command; the third adds the input flags, and the
    final option adds the internal input flags.

- **-c**

    Lists the capabilities of each engine.

- **-t**

    Tests if each specified engine is available, and displays the answer.

- **-tt**

    Displays an error trace for any unavailable engine.

- **-pre** _command_
- **-post** _command_

    Command-line configuration of engines.
    The **-pre** command is given to the engine before it is loaded and
    the **-post** command is given after the engine is loaded.
    The _command_ is of the form _cmd:val_ where _cmd_ is the command,
    and _val_ is the value for the command.
    See the example below.

# EXAMPLE

To list all the commands available to a dynamic engine:

    % gmssl engine -t -tt -vvvv dynamic
    (dynamic) Dynamic engine loading support
         [ unavailable ]
         SO_PATH: Specifies the path to the new ENGINE shared library
              (input flags): STRING
         NO_VCHECK: Specifies to continue even if version checking fails (boolean)
              (input flags): NUMERIC
         ID: Specifies an ENGINE id name for loading
              (input flags): STRING
         LIST_ADD: Whether to add a loaded ENGINE to the internal list (0=no,1=yes,2=mandatory)
              (input flags): NUMERIC
         DIR_LOAD: Specifies whether to load from 'DIR_ADD' directories (0=no,1=yes,2=mandatory)
              (input flags): NUMERIC
         DIR_ADD: Adds a directory from which ENGINEs can be loaded
              (input flags): STRING
         LOAD: Load up the ENGINE specified by other settings
              (input flags): NO_INPUT

To list the capabilities of the _rsax_ engine:

    % gmssl engine -c
    (rsax) RSAX engine support
     [RSA]
    (dynamic) Dynamic engine loading support

# COPYRIGHT

Copyright 2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the GmSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
