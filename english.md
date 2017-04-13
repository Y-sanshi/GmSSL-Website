## About GmSSL

GmSSL is an open source cryptographic toolbox that supports SM2 / SM3 / SM4 / SM9 and other national secret (national commercial password) algorithm, SM2 digital certificate and SM2 certificate based on SSL / TLS secure communication protocol to support the national security hardware password device , To provide in line with the national standard programming interface and command line tools, can be used to build PKI / CA, secure communication, data encryption and other standards in line with national security applications. The GmSSL project is a branch of the [OpenSSL](https://www.openssl.org)project and is compatible with OpenSSL. So GmSSL can replace the application of OpenSSL components, and make the application automatically with national security capabilities. The GmSSL project utilizes a business-friendly BSD open source license, open source and can be used for closed source commercial applications. GmSSL project by the Peking University [Guan Zhi](http://infosec.pku.edu.cn/~guanzhi/)deputy researcher of the cryptography research group development and maintenance, the project source code hosted in [GitHub](https://github.com /guanzhi/GmSSL). Since its release in 2014, GmSSL has been deployed and applied in multiple projects and products, and has won the second prize of the "One Cup" China Linux Software Contest in 2015 (the highest award) and [Open Source China](https://www.oschina.net/p/GmSSL) password class recommended items. The core goal of the GmSSL project is to promote the construction of cyberspace security through open source cryptography.

## Latest News

- March 2, 2017 GmSSL project registered its OID {iso(1) identified-organization(3) dod(6) internet(1) private(4) enterprise(1) GmSSL(49549)}
- February 12, 2017 Java wrapper support for full crypto library  [GmSSL-Java-Wrapper](http://gmssl.org/docs/java-api.html)
- January 18, 2017 Updated the project home page
- [More ...](http://gmssl.org/docs/changelog.html)

## Recent Plan

- On March 16, 2017, we start the integration work of Speck, Serpent and ZUC, and it would be completed in early April 2017.

   contributors [Simon](https://github.com/zhaoxiaomeng),[SuChao](https://github.com/GGSuchao),[LaiWei](https://github.com/laiwei360735),[HanShanxin](https://github.com/HanShanxin)

## SM Crypto Algorithm

The secret algorithm is the abbreviation of the national commercial cryptographic algorithm. Since 2012, the National Password Authority to the "People's Republic of China password industry standard" approach, have announced the SM2 / SM3 / SM4 and other cryptographic algorithm standards and application specifications. Which "SM" on behalf of "business secret", that is used for commercial, not involving state secrets of the password technology. SM2 is a public key cryptography algorithm based on elliptic curve cryptography, including digital signature, key exchange and public key encryption. It is used to replace international algorithms such as RSA / Diffie-Hellman / ECDSA / ECDH. SM3 is password hash algorithm, SM4 is a block cipher used to replace DES / AES and other international algorithms. SM9 is an identity-based cryptographic algorithm that can replace PKI / CA based on digital certificate. By deploying the secret algorithm, you can reduce the security risks caused by weak passwords and bug implementations and the overhead of deploying PKI / CA.

## Quick Start

Quick Start Guide describes the basic instructions for compiling, installing, and `gmssl` command line tools for GmSSL.

1. Download the source code ([zip](https://github.com/guanzhi/GmSSL/archive/master.zip))，unzip it to current directory.

   ```sh
   $ unzip GmSSL-master.zip
   ```

2. Compile and install

   Linux (Other platform see [Compile and instal](http://gmssl.org))

   ```sh
   $ ./config no-saf no-sdf no-skf no-sof no-zuc
   $ make
   $ sudo make install
   ```

   After installation, you can execute the `gmssl` command line tool to check for success

   ```sh
   $ gmssl version
   GmSSL 2.0 - OpenSSL 1.1.0d
   ```

3. SM4 encrypt file

   ```sh
   $ gmssl sms4 -e -in <yourfile> -out <yourfile>.sms4
   enter sms4-cbc encryption password: <your-password>
   Verifying - enter sms4-cbc encryption password: <your-password>
   ```

   decrypt

   ```sh
   $ gmssl sms4 -d -in <yourfile>.sms4
   enter sms4-cbc decryption password: <your-password>
   ```

4. Generate SM3 digest

   ```
   $ gmssl sm3 <yourfile>
   SM3(yourfile)= 66c7f0f462eeedd9d1f2d46bdc10e4e24167c4875cf2f7a2297da02b8f4ba8e0
   ```

5. Generate SM2 key and sign

   ```sh
   $ gmssl genpkey -algorithm EC -pkeyopt ec_paramgen_curve:sm2p256v1 \
                   -pkeyopt ec_param_enc:named_curve  -out signkey.pem
   $ gmssl pkeyutl -sign -pkeyopt ec_sign_algor:sm2 -inkey signkey.pem \
                   -in <yourfile> -out <yourfile>.sig
   ```

   You can export the public key from `signkey.pem` to the party that issued the signature

   ```sh
   $ gmssl pkey -pubout -in signkey.pem out vrfykey.pem
   $ gmssl pkeyutl -verify -pkeyopt ec_sign_algor:sm2 -inkey vrfykey.pem \
                   -in <yourfile> -sigfile <yourfile>.sig
   ```


   ## Project documentation

   - User manual

      * [Compile and install](http://gmssl.org/docs/install.html)

      * [Command line tool manual](http://gmssl.org/docs/commands.html)

      * [GmSSL EVP API](http://gmssl.org/docs/evp-api.html)

      * [GmSSL Java API](http://gmssl.org/docs/java-api.html)

   - Password algorithm

      * [SM1 group password](http://gmssl.org/docs/sm1.html)

      * [SSF33 group password](http://gmssl.org/docs/ssf33.html)

      * [SM2 elliptic curve public key password](http://gmssl.org/docs/sm2.html)

      * [SM3 password hash algorithm](http://gmssl.org/docs/sm3.html)

      * [SM4 / SMS4 group password](http://gmssl.org/docs/sm4.html)

      * [SM9 identity-based password](http://gmssl.org/docs/sm9.html)

      * [ZUC sequence password](http://gmssl.org/docs/zuc.html)

      * [CPK combination public key password](http://gmssl.org/docs/cpk.html)

      * [BF-IBE (Boneh-Franklin Identity-Based Encryption)](http://gmssl.org/docs/bfibe.html)

      * [BB-IBE (Boneh-Boyen Identity-Based Encryption)](http://gmssl.org/docs/bb1ibe.html)

   - password hardware

      * [Password hardware support](http://gmssl.org/docs/crypto-devices.html)

      * [Country density SKF password hardware](http://gmssl.org/docs/skf.html)

      * [National secret SDF password hardware](http://gmssl.org/docs/sdf.html)

      * [Key management service](http://gmssl.org/docs/keyservice.html)

   - Security protocol

      * [SSL / TLS protocol](http://gmssl.org/docs/ssl.html)

      * [National secret SSL VPN protocol](http://gmssl.org/docs/sslvpn.html)

      * [National secret IPSec VPN protocol](http://gmssl.org/docs/ipsecvpn.html)

   - Developer

      * [GmSSL Coding Style](http://gmssl.org/docs/gmssl-coding-style.html)

      * [Roadmap](http://gmssl.org/docs/roadmap.html)

      * [Open source license (GmSSL Licenses)](http://gmssl.org/docs/licenses.html)

   - Standards and norms

      * [People's Republic of China password industry standard](http://gmssl.org/docs/standards.html)

      * [National secret algorithm identification OID](http://gmssl.org/docs/oid.html)
