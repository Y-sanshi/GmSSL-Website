# NAME

X509\_VERIFY\_PARAM\_set\_flags, X509\_VERIFY\_PARAM\_clear\_flags,
X509\_VERIFY\_PARAM\_set\_flags, X509\_VERIFY\_PARAM\_clear\_flags,
X509\_VERIFY\_PARAM\_get\_flags, X509\_VERIFY\_PARAM\_set\_purpose,
X509\_VERIFY\_PARAM\_get\_inh\_flags, X509\_VERIFY\_PARAM\_set\_inh\_flags,
X509\_VERIFY\_PARAM\_set\_trust, X509\_VERIFY\_PARAM\_set\_depth,
X509\_VERIFY\_PARAM\_get\_depth, X509\_VERIFY\_PARAM\_set\_auth\_level,
X509\_VERIFY\_PARAM\_get\_auth\_level, X509\_VERIFY\_PARAM\_set\_time,
X509\_VERIFY\_PARAM\_get\_time,
X509\_VERIFY\_PARAM\_add0\_policy, X509\_VERIFY\_PARAM\_set1\_policies,
X509\_VERIFY\_PARAM\_set1\_host, X509\_VERIFY\_PARAM\_add1\_host,
X509\_VERIFY\_PARAM\_set\_hostflags, X509\_VERIFY\_PARAM\_get0\_peername,
X509\_VERIFY\_PARAM\_set1\_email, X509\_VERIFY\_PARAM\_set1\_ip,
X509\_VERIFY\_PARAM\_set1\_ip\_asc
\- X509 verification parameters

# SYNOPSIS

    #include <openssl/x509_vfy.h>

    int X509_VERIFY_PARAM_set_flags(X509_VERIFY_PARAM *param,
                                    unsigned long flags);
    int X509_VERIFY_PARAM_clear_flags(X509_VERIFY_PARAM *param,
                                      unsigned long flags);
    unsigned long X509_VERIFY_PARAM_get_flags(X509_VERIFY_PARAM *param);

    int X509_VERIFY_PARAM_set_inh_flags(X509_VERIFY_PARAM *param,
                                        uint32_t flags);
    uint32_t X509_VERIFY_PARAM_get_inh_flags(const X509_VERIFY_PARAM *param);

    int X509_VERIFY_PARAM_set_purpose(X509_VERIFY_PARAM *param, int purpose);
    int X509_VERIFY_PARAM_set_trust(X509_VERIFY_PARAM *param, int trust);

    void X509_VERIFY_PARAM_set_time(X509_VERIFY_PARAM *param, time_t t);
    time_t X509_VERIFY_PARAM_get_time(const X509_VERIFY_PARAM *param);

    int X509_VERIFY_PARAM_add0_policy(X509_VERIFY_PARAM *param,
                                           ASN1_OBJECT *policy);
    int X509_VERIFY_PARAM_set1_policies(X509_VERIFY_PARAM *param,
                                           STACK_OF(ASN1_OBJECT) *policies);

    void X509_VERIFY_PARAM_set_depth(X509_VERIFY_PARAM *param, int depth);
    int X509_VERIFY_PARAM_get_depth(const X509_VERIFY_PARAM *param);

    void X509_VERIFY_PARAM_set_auth_level(X509_VERIFY_PARAM *param,
                                           int auth_level);
    int X509_VERIFY_PARAM_get_auth_level(const X509_VERIFY_PARAM *param);

    int X509_VERIFY_PARAM_set1_host(X509_VERIFY_PARAM *param,
                                    const char *name, size_t namelen);
    int X509_VERIFY_PARAM_add1_host(X509_VERIFY_PARAM *param,
                                    const char *name, size_t namelen);
    void X509_VERIFY_PARAM_set_hostflags(X509_VERIFY_PARAM *param,
                                         unsigned int flags);
    char *X509_VERIFY_PARAM_get0_peername(X509_VERIFY_PARAM *param);
    int X509_VERIFY_PARAM_set1_email(X509_VERIFY_PARAM *param,
                                    const char *email, size_t emaillen);
    int X509_VERIFY_PARAM_set1_ip(X509_VERIFY_PARAM *param,
                                  const unsigned char *ip, size_t iplen);
    int X509_VERIFY_PARAM_set1_ip_asc(X509_VERIFY_PARAM *param, const char *ipasc);

# DESCRIPTION

These functions manipulate the **X509\_VERIFY\_PARAM** structure associated with
a certificate verification operation.

The X509\_VERIFY\_PARAM\_set\_flags() function sets the flags in **param** by oring
it with **flags**. See the **VERIFICATION FLAGS** section for a complete
description of values the **flags** parameter can take.

X509\_VERIFY\_PARAM\_get\_flags() returns the flags in **param**.

X509\_VERIFY\_PARAM\_get\_inh\_flags() returns the inheritance flags in **param**
which specifies how verification flags are copied from one structure to
another. X509\_VERIFY\_PARAM\_set\_inh\_flags() sets the inheritance flags.
See the **INHERITANCE FLAGS** section for a description of these bits.

X509\_VERIFY\_PARAM\_clear\_flags() clears the flags **flags** in **param**.

X509\_VERIFY\_PARAM\_set\_purpose() sets the verification purpose in **param**
to **purpose**. This determines the acceptable purpose of the certificate
chain, for example SSL client or SSL server.

X509\_VERIFY\_PARAM\_set\_trust() sets the trust setting in **param** to
**trust**.

X509\_VERIFY\_PARAM\_set\_time() sets the verification time in **param** to
**t**. Normally the current time is used.

X509\_VERIFY\_PARAM\_add0\_policy() enables policy checking (it is disabled
by default) and adds **policy** to the acceptable policy set.

X509\_VERIFY\_PARAM\_set1\_policies() enables policy checking (it is disabled
by default) and sets the acceptable policy set to **policies**. Any existing
policy set is cleared. The **policies** parameter can be **NULL** to clear
an existing policy set.

X509\_VERIFY\_PARAM\_set\_depth() sets the maximum verification depth to **depth**.
That is the maximum number of intermediate CA certificates that can appear in a
chain.
A maximal depth chain contains 2 more certificates than the limit, since
neither the end-entity certificate nor the trust-anchor count against this
limit.
Thus a **depth** limit of 0 only allows the end-entity certificate to be signed
directly by the trust-anchor, while with a **depth** limit of 1 there can be one
intermediate CA certificate between the trust-anchor and the end-entity
certificate.

X509\_VERIFY\_PARAM\_set\_auth\_level() sets the authentication security level to
**auth\_level**.
The authentication security level determines the acceptable signature and public
key strength when verifying certificate chains.
For a certificate chain to validate, the public keys of all the certificates
must meet the specified security level.
The signature algorithm security level is not enforced for the chain's _trust
anchor_ certificate, which is either directly trusted or validated by means other
than its signature.
See [SSL\_CTX\_set\_security\_level(3)](http://man.he.net/man3/SSL_CTX_set_security_level) for the definitions of the available
levels.
The default security level is -1, or "not set".
At security level 0 or lower all algorithms are acceptable.
Security level 1 requires at least 80-bit-equivalent security and is broadly
interoperable, though it will, for example, reject MD5 signatures or RSA keys
shorter than 1024 bits.

X509\_VERIFY\_PARAM\_set1\_host() sets the expected DNS hostname to
**name** clearing any previously specified host name or names.  If
**name** is NULL, or empty the list of hostnames is cleared, and
name checks are not performed on the peer certificate.  If **name**
is NUL-terminated, **namelen** may be zero, otherwise **namelen**
must be set to the length of **name**.  When a hostname is specified,
certificate verification automatically invokes [X509\_check\_host(3)](http://man.he.net/man3/X509_check_host)
with flags equal to the **flags** argument given to
X509\_VERIFY\_PARAM\_set\_hostflags() (default zero).  Applications
are strongly advised to use this interface in preference to explicitly
calling [X509\_check\_host(3)](http://man.he.net/man3/X509_check_host), hostname checks are out of scope
with the DANE-EE(3) certificate usage, and the internal check will
be suppressed as appropriate when DANE support is added to OpenSSL.

X509\_VERIFY\_PARAM\_add1\_host() adds **name** as an additional reference
identifier that can match the peer's certificate.  Any previous names
set via X509\_VERIFY\_PARAM\_set1\_host() or X509\_VERIFY\_PARAM\_add1\_host()
are retained, no change is made if **name** is NULL or empty.  When
multiple names are configured, the peer is considered verified when
any name matches.

X509\_VERIFY\_PARAM\_get0\_peername() returns the DNS hostname or subject
CommonName from the peer certificate that matched one of the reference
identifiers.  When wildcard matching is not disabled, or when a
reference identifier specifies a parent domain (starts with ".")
rather than a hostname, the peer name may be a wildcard name or a
sub-domain of the reference identifier respectively.  The return
string is allocated by the library and is no longer valid once the
associated **param** argument is freed.  Applications must not free
the return value.

X509\_VERIFY\_PARAM\_set1\_email() sets the expected RFC822 email address to
**email**.  If **email** is NUL-terminated, **emaillen** may be zero, otherwise
**emaillen** must be set to the length of **email**.  When an email address
is specified, certificate verification automatically invokes
[X509\_check\_email(3)](http://man.he.net/man3/X509_check_email).

X509\_VERIFY\_PARAM\_set1\_ip() sets the expected IP address to **ip**.
The **ip** argument is in binary format, in network byte-order and
**iplen** must be set to 4 for IPv4 and 16 for IPv6.  When an IP
address is specified, certificate verification automatically invokes
[X509\_check\_ip(3)](http://man.he.net/man3/X509_check_ip).

X509\_VERIFY\_PARAM\_set1\_ip\_asc() sets the expected IP address to
**ipasc**.  The **ipasc** argument is a NUL-terminal ASCII string:
dotted decimal quad for IPv4 and colon-separated hexadecimal for
IPv6.  The condensed "::" notation is supported for IPv6 addresses.

# RETURN VALUES

X509\_VERIFY\_PARAM\_set\_flags(), X509\_VERIFY\_PARAM\_clear\_flags(),
X509\_VERIFY\_PARAM\_set\_inh\_flags(),
X509\_VERIFY\_PARAM\_set\_purpose(), X509\_VERIFY\_PARAM\_set\_trust(),
X509\_VERIFY\_PARAM\_add0\_policy() X509\_VERIFY\_PARAM\_set1\_policies(),
X509\_VERIFY\_PARAM\_set1\_host(), X509\_VERIFY\_PARAM\_add1\_host(),
X509\_VERIFY\_PARAM\_set1\_email(), X509\_VERIFY\_PARAM\_set1\_ip() and
X509\_VERIFY\_PARAM\_set1\_ip\_asc() return 1 for success and 0 for
failure.

X509\_VERIFY\_PARAM\_get\_flags() returns the current verification flags.

X509\_VERIFY\_PARAM\_get\_inh\_flags() returns the current inheritance flags.

X509\_VERIFY\_PARAM\_set\_time() and X509\_VERIFY\_PARAM\_set\_depth() do not return
values.

X509\_VERIFY\_PARAM\_get\_depth() returns the current verification depth.

X509\_VERIFY\_PARAM\_get\_auth\_level() returns the current authentication security
level.

# VERIFICATION FLAGS

The verification flags consists of zero or more of the following flags
ored together.

**X509\_V\_FLAG\_CRL\_CHECK** enables CRL checking for the certificate chain leaf
certificate. An error occurs if a suitable CRL cannot be found.

**X509\_V\_FLAG\_CRL\_CHECK\_ALL** enables CRL checking for the entire certificate
chain.

**X509\_V\_FLAG\_IGNORE\_CRITICAL** disabled critical extension checking. By default
any unhandled critical extensions in certificates or (if checked) CRLs results
in a fatal error. If this flag is set unhandled critical extensions are
ignored. **WARNING** setting this option for anything other than debugging
purposes can be a security risk. Finer control over which extensions are
supported can be performed in the verification callback.

The **X509\_V\_FLAG\_X509\_STRICT** flag disables workarounds for some broken
certificates and makes the verification strictly apply **X509** rules.

**X509\_V\_FLAG\_ALLOW\_PROXY\_CERTS** enables proxy certificate verification.

**X509\_V\_FLAG\_POLICY\_CHECK** enables certificate policy checking, by default
no policy checking is performed. Additional information is sent to the
verification callback relating to policy checking.

**X509\_V\_FLAG\_EXPLICIT\_POLICY**, **X509\_V\_FLAG\_INHIBIT\_ANY** and
**X509\_V\_FLAG\_INHIBIT\_MAP** set the **require explicit policy**, **inhibit any
policy** and **inhibit policy mapping** flags respectively as defined in
**RFC3280**. Policy checking is automatically enabled if any of these flags
are set.

If **X509\_V\_FLAG\_NOTIFY\_POLICY** is set and the policy checking is successful
a special status code is set to the verification callback. This permits it
to examine the valid policy tree and perform additional checks or simply
log it for debugging purposes.

By default some additional features such as indirect CRLs and CRLs signed by
different keys are disabled. If **X509\_V\_FLAG\_EXTENDED\_CRL\_SUPPORT** is set
they are enabled.

If **X509\_V\_FLAG\_USE\_DELTAS** is set delta CRLs (if present) are used to
determine certificate status. If not set deltas are ignored.

**X509\_V\_FLAG\_CHECK\_SS\_SIGNATURE** enables checking of the root CA self signed
certificate signature. By default this check is disabled because it doesn't
add any additional security but in some cases applications might want to
check the signature anyway. A side effect of not checking the root CA
signature is that disabled or unsupported message digests on the root CA
are not treated as fatal errors.

If **X509\_V\_FLAG\_TRUSTED\_FIRST** is set, when constructing the certificate chain,
[X509\_verify\_cert(3)](http://man.he.net/man3/X509_verify_cert) will search the trust store for issuer certificates before
searching the provided untrusted certificates.
As of OpenSSL 1.1.0 this option is on by default and cannot be disabled.

The **X509\_V\_FLAG\_NO\_ALT\_CHAINS** flag suppresses checking for alternative
chains.
By default, unless **X509\_V\_FLAG\_TRUSTED\_FIRST** is set, when building a
certificate chain, if the first certificate chain found is not trusted, then
OpenSSL will attempt to replace untrusted certificates supplied by the peer
with certificates from the trust store to see if an alternative chain can be
found that is trusted.
As of OpenSSL 1.1.0, with **X509\_V\_FLAG\_TRUSTED\_FIRST** always set, this option
has no effect.

The **X509\_V\_FLAG\_NO\_CHECK\_TIME** flag suppresses checking the validity period
of certificates and CRLs against the current time. If X509\_VERIFY\_PARAM\_set\_time()
is used to specify a verification time, the check is not suppressed.

# INHERITANCE FLAGS

These flags spevify how parameters are "inherited" from one structure to
another.

If **X509\_VP\_FLAG\_ONCE** is set then the current setting is zeroed
after the next call.

If **X509\_VP\_FLAG\_LOCKED** is set then no values are copied.  This overrides
all of the following flags.

If **X509\_VP\_FLAG\_DEFAULT** is set then anything set in the source is copied
to the destination. Effectively the values in "to" become default values
which will be used only if nothing new is set in "from".  This is the
default.

If **X509\_VP\_FLAG\_OVERWRITE** is set then all value are copied across whether
they are set or not. Flags is still Ored though.

If **X509\_VP\_FLAG\_RESET\_FLAGS** is set then the flags value is copied instead
of ORed.

# NOTES

The above functions should be used to manipulate verification parameters
instead of legacy functions which work in specific structures such as
X509\_STORE\_CTX\_set\_flags().

# BUGS

Delta CRL checking is currently primitive. Only a single delta can be used and
(partly due to limitations of **X509\_STORE**) constructed CRLs are not
maintained.

If CRLs checking is enable CRLs are expected to be available in the
corresponding **X509\_STORE** structure. No attempt is made to download
CRLs from the CRL distribution points extension.

# EXAMPLE

Enable CRL checking when performing certificate verification during SSL
connections associated with an **SSL\_CTX** structure **ctx**:

    X509_VERIFY_PARAM *param;
    param = X509_VERIFY_PARAM_new();
    X509_VERIFY_PARAM_set_flags(param, X509_V_FLAG_CRL_CHECK);
    SSL_CTX_set1_param(ctx, param);
    X509_VERIFY_PARAM_free(param);

# SEE ALSO

[X509\_verify\_cert(3)](http://man.he.net/man3/X509_verify_cert),
[X509\_check\_host(3)](http://man.he.net/man3/X509_check_host),
[X509\_check\_email(3)](http://man.he.net/man3/X509_check_email),
[X509\_check\_ip(3)](http://man.he.net/man3/X509_check_ip)

# HISTORY

The **X509\_V\_FLAG\_NO\_ALT\_CHAINS** flag was added in OpenSSL 1.1.0
The legacy **X509\_V\_FLAG\_CB\_ISSUER\_CHECK** flag is deprecated as of
OpenSSL 1.1.0, and has no effect.

# COPYRIGHT

Copyright 2009-2016 The OpenSSL Project Authors. All Rights Reserved.

Licensed under the OpenSSL license (the "License").  You may not use
this file except in compliance with the License.  You can obtain a copy
in the file LICENSE in the source distribution or at
[https://www.openssl.org/source/license.html](https://www.openssl.org/source/license.html).
