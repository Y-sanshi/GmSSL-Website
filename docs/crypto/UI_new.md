# NAME

UI, UI\_METHOD,
UI\_new, UI\_new\_method, UI\_free, UI\_add\_input\_string, UI\_dup\_input\_string,
UI\_add\_verify\_string, UI\_dup\_verify\_string, UI\_add\_input\_boolean,
UI\_dup\_input\_boolean, UI\_add\_info\_string, UI\_dup\_info\_string,
UI\_add\_error\_string, UI\_dup\_error\_string, UI\_construct\_prompt,
UI\_add\_user\_data, UI\_get0\_user\_data, UI\_get0\_result, UI\_process,
UI\_ctrl, UI\_set\_default\_method, UI\_get\_default\_method, UI\_get\_method,
UI\_set\_method, UI\_OpenSSL, - user interface

# SYNOPSIS

    #include <openssl/ui.h>

    typedef struct ui_st UI;
    typedef struct ui_method_st UI_METHOD;

    UI *UI_new(void);
    UI *UI_new_method(const UI_METHOD *method);
    void UI_free(UI *ui);

    int UI_add_input_string(UI *ui, const char *prompt, int flags,
           char *result_buf, int minsize, int maxsize);
    int UI_dup_input_string(UI *ui, const char *prompt, int flags,
           char *result_buf, int minsize, int maxsize);
    int UI_add_verify_string(UI *ui, const char *prompt, int flags,
           char *result_buf, int minsize, int maxsize, const char *test_buf);
    int UI_dup_verify_string(UI *ui, const char *prompt, int flags,
           char *result_buf, int minsize, int maxsize, const char *test_buf);
    int UI_add_input_boolean(UI *ui, const char *prompt, const char *action_desc,
           const char *ok_chars, const char *cancel_chars,
           int flags, char *result_buf);
    int UI_dup_input_boolean(UI *ui, const char *prompt, const char *action_desc,
           const char *ok_chars, const char *cancel_chars,
           int flags, char *result_buf);
    int UI_add_info_string(UI *ui, const char *text);
    int UI_dup_info_string(UI *ui, const char *text);
    int UI_add_error_string(UI *ui, const char *text);
    int UI_dup_error_string(UI *ui, const char *text);

    char *UI_construct_prompt(UI *ui_method,
           const char *object_desc, const char *object_name);

    void *UI_add_user_data(UI *ui, void *user_data);
    void *UI_get0_user_data(UI *ui);

    const char *UI_get0_result(UI *ui, int i);

    int UI_process(UI *ui);

    int UI_ctrl(UI *ui, int cmd, long i, void *p, void (*f)());

    void UI_set_default_method(const UI_METHOD *meth);
    const UI_METHOD *UI_get_default_method(void);
    const UI_METHOD *UI_get_method(UI *ui);
    const UI_METHOD *UI_set_method(UI *ui, const UI_METHOD *meth);

    UI_METHOD *UI_OpenSSL(void);

# DESCRIPTION

UI stands for User Interface, and is general purpose set of routines to
prompt the user for text-based information.  Through user-written methods
(see [ui\_create(3)](http://man.he.net/man3/ui_create)), prompting can be done in any way
imaginable, be it plain text prompting, through dialog boxes or from a
cell phone.

All the functions work through a context of the type UI.  This context
contains all the information needed to prompt correctly as well as a
reference to a UI\_METHOD, which is an ordered vector of functions that
carry out the actual prompting.

The first thing to do is to create a UI with UI\_new() or UI\_new\_method(),
then add information to it with the UI\_add or UI\_dup functions.  Also,
user-defined random data can be passed down to the underlying method
through calls to UI\_add\_user\_data.  The default UI method doesn't care
about these data, but other methods might.  Finally, use UI\_process()
to actually perform the prompting and UI\_get0\_result() to find the result
to the prompt.

A UI can contain more than one prompt, which are performed in the given
sequence.  Each prompt gets an index number which is returned by the
UI\_add and UI\_dup functions, and has to be used to get the corresponding
result with UI\_get0\_result().

The functions are as follows:

UI\_new() creates a new UI using the default UI method.  When done with
this UI, it should be freed using UI\_free().

UI\_new\_method() creates a new UI using the given UI method.  When done with
this UI, it should be freed using UI\_free().

UI\_OpenSSL() returns the built-in UI method (note: not the default one,
since the default can be changed.  See further on).  This method is the
most machine/OS dependent part of OpenSSL and normally generates the
most problems when porting.

UI\_free() removes a UI from memory, along with all other pieces of memory
that's connected to it, like duplicated input strings, results and others.
If **ui** is NULL nothing is done.

UI\_add\_input\_string() and UI\_add\_verify\_string() add a prompt to the UI,
as well as flags and a result buffer and the desired minimum and maximum
sizes of the result, not counting the final NUL character.  The given
information is used to prompt for information, for example a password,
and to verify a password (i.e. having the user enter it twice and check
that the same string was entered twice).  UI\_add\_verify\_string() takes
and extra argument that should be a pointer to the result buffer of the
input string that it's supposed to verify, or verification will fail.

UI\_add\_input\_boolean() adds a prompt to the UI that's supposed to be answered
in a boolean way, with a single character for yes and a different character
for no.  A set of characters that can be used to cancel the prompt is given
as well.  The prompt itself is divided in two, one part being the
descriptive text (given through the _prompt_ argument) and one describing
the possible answers (given through the _action\_desc_ argument).

UI\_add\_info\_string() and UI\_add\_error\_string() add strings that are shown at
the same time as the prompt for extra information or to show an error string.
The difference between the two is only conceptual.  With the builtin method,
there's no technical difference between them.  Other methods may make a
difference between them, however.

The flags currently supported are **UI\_INPUT\_FLAG\_ECHO**, which is relevant for
UI\_add\_input\_string() and will have the users response be echoed (when
prompting for a password, this flag should obviously not be used, and
**UI\_INPUT\_FLAG\_DEFAULT\_PWD**, which means that a default password of some
sort will be used (completely depending on the application and the UI
method).

UI\_dup\_input\_string(), UI\_dup\_verify\_string(), UI\_dup\_input\_boolean(),
UI\_dup\_info\_string() and UI\_dup\_error\_string() are basically the same
as their UI\_add counterparts, except that they make their own copies
of all strings.

UI\_construct\_prompt() is a helper function that can be used to create
a prompt from two pieces of information: an description and a name.
The default constructor (if there is none provided by the method used)
creates a string "Enter _description_ for _name_:".  With the
description "pass phrase" and the file name "foo.key", that becomes
"Enter pass phrase for foo.key:".  Other methods may create whatever
string and may include encodings that will be processed by the other
method functions.

UI\_add\_user\_data() adds a piece of memory for the method to use at any
time.  The builtin UI method doesn't care about this info.  Note that several
calls to this function doesn't add data, it replaces the previous blob
with the one given as argument.

UI\_get0\_user\_data() retrieves the data that has last been given to the
UI with UI\_add\_user\_data().

UI\_get0\_result() returns a pointer to the result buffer associated with
the information indexed by _i_.

UI\_process() goes through the information given so far, does all the printing
and prompting and returns.

UI\_ctrl() adds extra control for the application author.  For now, it
understands two commands: **UI\_CTRL\_PRINT\_ERRORS**, which makes UI\_process()
print the OpenSSL error stack as part of processing the UI, and
**UI\_CTRL\_IS\_REDOABLE**, which returns a flag saying if the used UI can
be used again or not.

UI\_set\_default\_method() changes the default UI method to the one given.

UI\_get\_default\_method() returns a pointer to the current default UI method.

UI\_get\_method() returns the UI method associated with a given UI.

UI\_set\_method() changes the UI method associated with a given UI.

# COPYRIGHT

Copyright 2001-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
