# EVP API

[TOC]

## 密钥和参数生成

如果`EVP_PKEY`对象需要，EVP函数支持生成参数和密钥的功能。由于这些函数使用随机数，因此应当就像这里所讨论的确保随机数生成器采用适当的种子。

### 参数生成
仅支持以下`EVP_PKEY`类型的参数生成：

* `EVP_PKEY_EC` (用于SM2和椭圆曲线ECDSA、ECIES、ECDH密钥)
* `EVP_PKEY_DSA`
* `EVP_PKEY_DH`

以下的示例代码演示了如何为每种密钥类型产生参数的例子：


```c
/* 创建用于生成参数的上下文 */
if(!(pctx = EVP_PKEY_CTX_new_id(type, NULL))) goto err;
if(!EVP_PKEY_paramgen_init(pctx)) goto err;

/* 根据类型设置paramgen参数 */
switch(type)
{
case EVP_PKEY_EC:
	/* 使用在obj_mac.h中定义的NID_sm2p256v1命名曲线 */
	if(!EVP_PKEY_CTX_set_ec_paramgen_curve_nid(pctx, NID_sm2p256v1)) goto err;
	break;

case EVP_PKEY_DSA:
	/* 设置位长度为2048*/
	if(!EVP_PKEY_CTX_set_dsa_paramgen_bits(pctx, 2048)) goto err;
	break;

case EVP_PKEY_DH:
	/* 设置一个2048位的素数 */
	if(!EVP_PKEY_CTX_set_dh_paramgen_prime_len(pctx, 2048)) goto err;
}

/* 生成参数 */
if (!EVP_PKEY_paramgen(pctx, &params)) goto err;
```

### 密钥生成

以下示例代码演示了如何生成除`EVP_PKEY HMAC`和`EVP_PKEY_CTX`密钥之外的密钥的例子：

```c
if(*params != NULL) {
	if(!(kctx = EVP_PKEY_CTX_new(params, NULL))) goto err;
} else {
	/* 创建用于生成密钥的上下文 */
	if(!(kctx = EVP_PKEY_CTX_new_id(type, NULL))) goto err;
}

if(!EVP_PKEY_keygen_init(kctx)) goto err;

/* RSA密钥是在生成密钥期间设置密钥长度，而不是在生成参数的时候! */
if(type == EVP_PKEY_RSA) {
	if(!EVP_PKEY_CTX_set_rsa_keygen_bits(kctx, 2048)) goto err;
}

/* 生成密钥 */
if (!EVP_PKEY_keygen(kctx, &key)) goto err;
```

CMAC密钥以类似的方式生成

```c
if(!(kctx = EVP_PKEY_CTX_new_id(type, NULL))) goto err;

if(!EVP_PKEY_keygen_init(kctx)) goto err;

/* 设置CMAC使用的密码 */
if (EVP_PKEY_CTX_ctrl(kctx, -1, EVP_PKEY_OP_KEYGEN, 
	EVP_PKEY_CTRL_CIPHER, 
	0, (void *)EVP_aes_256_ecb()) <= 0)
	goto err;

/* 设置CMAC要用的密钥数据 */
if (EVP_PKEY_CTX_ctrl(kctx, -1, EVP_PKEY_OP_KEYGEN,
	EVP_PKEY_CTRL_SET_MAC_KEY,
	/*密钥长度*/32, "01234567890123456789012345678901") <= 0)
	goto err;

/* 生成密钥 */
if (!EVP_PKEY_keygen(kctx, &key)) goto err;
```

HMAC密钥可以以与CMAC密钥相同的方式生成，但是不需要采用密码。封装了此过程的一个方便的函数简化了HMAC密钥的生成过程：

```c
key = EVP_PKEY_new_mac_key(EVP_PKEY_HMAC, NULL, "password", strlen("password"));
```
## Diffie-Hellman 参数

为了使用完全正向保密加密算法，必须设置 Diffie-Hellman参数(在服务器端)，或者PFS(完全正向保密)加密算法被没有任何提示地忽略掉。

>内容

>1 Diffie-Hellman

>2 验证参数

>3 椭圆曲线 Diffie-Hellman

>4 RFC 3526 PEM 编码组

### Diffie-Hellman

`SSL_CTX_set_tmp_dh` 用来设置上下文的Diffie-Hellman参数。使用该函数获得Diffie-Hellman参数的最简单的方法之一就是使用带有-C选项的dhparam命令行程序生成随机的Diffie-Hellman参数，并将生成的代码片段嵌入程序中。例如，`openssl dhparam -C 2236`可能导致：

```c
#ifndef HEADER_DH_H
#include <openssl/dh.h>
#endif
DH *get_dh2236()
	{
	static unsigned char dh2236_p[]={
		0x0F,0x52,0xE5,0x24,0xF5,0xFA,0x9D,0xDC,0xC6,0xAB,0xE6,0x04,
		0xE4,0x20,0x89,0x8A,0xB4,0xBF,0x27,0xB5,0x4A,0x95,0x57,0xA1,
		0x06,0xE7,0x30,0x73,0x83,0x5E,0xC9,0x23,0x11,0xED,0x42,0x45,
		0xAC,0x49,0xD3,0xE3,0xF3,0x34,0x73,0xC5,0x7D,0x00,0x3C,0x86,
		0x63,0x74,0xE0,0x75,0x97,0x84,0x1D,0x0B,0x11,0xDA,0x04,0xD0,
		0xFE,0x4F,0xB0,0x37,0xDF,0x57,0x22,0x2E,0x96,0x42,0xE0,0x7C,
		0xD7,0x5E,0x46,0x29,0xAF,0xB1,0xF4,0x81,0xAF,0xFC,0x9A,0xEF,
		0xFA,0x89,0x9E,0x0A,0xFB,0x16,0xE3,0x8F,0x01,0xA2,0xC8,0xDD,
		0xB4,0x47,0x12,0xF8,0x29,0x09,0x13,0x6E,0x9D,0xA8,0xF9,0x5D,
		0x08,0x00,0x3A,0x8C,0xA7,0xFF,0x6C,0xCF,0xE3,0x7C,0x3B,0x6B,
		0xB4,0x26,0xCC,0xDA,0x89,0x93,0x01,0x73,0xA8,0x55,0x3E,0x5B,
		0x77,0x25,0x8F,0x27,0xA3,0xF1,0xBF,0x7A,0x73,0x1F,0x85,0x96,
		0x0C,0x45,0x14,0xC1,0x06,0xB7,0x1C,0x75,0xAA,0x10,0xBC,0x86,
		0x98,0x75,0x44,0x70,0xD1,0x0F,0x20,0xF4,0xAC,0x4C,0xB3,0x88,
		0x16,0x1C,0x7E,0xA3,0x27,0xE4,0xAD,0xE1,0xA1,0x85,0x4F,0x1A,
		0x22,0x0D,0x05,0x42,0x73,0x69,0x45,0xC9,0x2F,0xF7,0xC2,0x48,
		0xE3,0xCE,0x9D,0x74,0x58,0x53,0xE7,0xA7,0x82,0x18,0xD9,0x3D,
		0xAF,0xAB,0x40,0x9F,0xAA,0x4C,0x78,0x0A,0xC3,0x24,0x2D,0xDB,
		0x12,0xA9,0x54,0xE5,0x47,0x87,0xAC,0x52,0xFE,0xE8,0x3D,0x0B,
		0x56,0xED,0x9C,0x9F,0xFF,0x39,0xE5,0xE5,0xBF,0x62,0x32,0x42,
		0x08,0xAE,0x6A,0xED,0x88,0x0E,0xB3,0x1A,0x4C,0xD3,0x08,0xE4,
		0xC4,0xAA,0x2C,0xCC,0xB1,0x37,0xA5,0xC1,0xA9,0x64,0x7E,0xEB,
		0xF9,0xD3,0xF5,0x15,0x28,0xFE,0x2E,0xE2,0x7F,0xFE,0xD9,0xB9,
		0x38,0x42,0x57,0x03,
		};
	static unsigned char dh2236_g[]={
		0x02,
		};
	DH *dh;
	
	if ((dh=DH_new()) == NULL) return(NULL);
	dh->p=BN_bin2bn(dh2236_p,sizeof(dh2236_p),NULL);
	dh->g=BN_bin2bn(dh2236_g,sizeof(dh2236_g),NULL);
	if ((dh->p == NULL) || (dh->g == NULL))
		{ DH_free(dh); return(NULL); }
	return(dh);
	}
```

然后也可以这样用：

```c
DH *dh = get_dh2236 ();
if (1 != SSL_CTX_set_tmp_dh (ctx, dh))
  	error ();
DH_free (dh);
```

请确保选择适合您想要实现的安全级别的位长度，但是请记住Diffie-Hellman参数如果长于2236位可能会与旧版本的NSS不兼容。更糟的是,1.7之前的Java版本不支持长于1024位的Diffie-Hellman参数!

### 验证参数

Diffie-Hellman参数在加载后需要进行验证。要执行参数验证，需要调用`DH_check`。`DH_check`返回0或者以下的掩码位：

* `DH_CHECK_P_NOT_PRIME (0x01)`
* `DH_CHECK_P_NOT_SAFE_PRIME (0x02)`
* `DH_UNABLE_TO_CHECK_GENERATOR (0x04)`
* `DH_NOT_SUITABLE_GENERATOR (0x08)`

验证代码可能看起来像下面这样(为了清楚起见，省略了错误检查)：

```c
BIO* bio = ...;
DH* dh = PEM_read_bio_DHparams(bio, NULL, NULL, NULL);

int rc, codes = 0;
rc = DH_check(dh, &codes);
assert(rc == 1);

if(BN_is_word(dh->g, DH_GENERATOR_2))
{
    long residue = BN_mod_word(dh->p, 24);
    if(residue == 11 || residue == 23) {
        codes &= ~DH_NOT_SUITABLE_GENERATOR;
    }
}

if (codes & DH_UNABLE_TO_CHECK_GENERATOR)
    printf("DH_check: failed to test generator\n");

if (codes & DH_NOT_SUITABLE_GENERATOR)
    printf("DH_check: g is not a suitable generator\n");

if (codes & DH_CHECK_P_NOT_PRIME)
    printf("DH_check: p is not prime\n");

if (codes & DH_CHECK_P_NOT_SAFE_PRIME)
    printf("DH_check: p is not a safe prime\n");
```

执行对`BN_mod_word`(dh->p, 24)的额外调用(以及取消对`DH_NOT_SUITABLE_GENERATOR`的屏蔽)，以确保程序接受IETF组参数。当g = 2时，OpenSSL检查质数是否等于11；而当g = 2时，IETF的质数等于23。如果没有测试，IETF参数无法进行验证。有关的详细信息，请参阅Diffie-Hellman参数检查(当g = 2时，必须p mod 24 == 11?)。

### 椭圆曲线 Diffie-Hellman

对于椭圆曲线Diffie-Hellman,可以这样做：

```c
EC_KEY *ecdh = EC_KEY_new_by_curve_name (NID_X9_62_prime256v1);
if (! ecdh)
  	error ();
if (1 != SSL_CTX_set_tmp_ecdh (ctx, ecdh))
  	error ();
EC_KEY_free (ecdh);
```

或者在OpenSSL 1.0.2版本(截至2013年2月尚未发布)以及更高的版本中，应该可以这样做：

```c
SSL_CTX_set_ecdh_auto (ctx, 1)
```

有关的更多信息，请参阅椭圆曲线Diffie-Hellman和椭圆曲线加密。

### RFC 3526 PEM 编码组

下面是在RFC 3526，用于因特网密钥交换(IKE)的更多模块化指数(MODP)Diffie-Hellman组(1024位的参数来自RFC 2409)指定的三个Diffie-Hellman MODP组。它们可以与`PEM_read_bio_DHparams`和BIO内存一起使用。RFC 3526还提供了1536位，6144位和8192位的质数。

```c	
static const char g_dh1024_sz[] =
    "-----BEGIN DH PARAMETERS-----\n"
    "MIGHAoGBAP//////////yQ/aoiFowjTExmKLgNwc0SkCTgiKZ8x0Agu+pjsTmyJR\n"
    "Sgh5jjQE3e+VGbPNOkMbMCsKbfJfFDdP4TVtbVHCReSFtXZiXn7G9ExC6aY37WsL\n"
    "/1y29Aa37e44a/taiZ+lrp8kEXxLH+ZJKGZR7OZTgf//////////AgEC\n"
    "-----END DH PARAMETERS-----";

static const char g_dh1536_sz[] = "-----BEGIN DH PARAMETERS-----\n"
    "MIHHAoHBAP//////////yQ/aoiFowjTExmKLgNwc0SkCTgiKZ8x0Agu+pjsTmyJR\n"
    "Sgh5jjQE3e+VGbPNOkMbMCsKbfJfFDdP4TVtbVHCReSFtXZiXn7G9ExC6aY37WsL\n"
    "/1y29Aa37e44a/taiZ+lrp8kEXxLH+ZJKGZR7ORbPcIAfLihY78FmNpINhxV05pp\n"
    "Fj+o/STPX4NlXSPco62WHGLzViCFUrue1SkHcJaWbWcMNU5KvJgE8XRsCMojcyf/\n"
    "/////////wIBAg==\n"
    "-----END DH PARAMETERS-----";

static const char g_dh2048_sz[] =
    "-----BEGIN DH PARAMETERS-----\n"
    "MIIBCAKCAQEA///////////JD9qiIWjCNMTGYouA3BzRKQJOCIpnzHQCC76mOxOb\n"
    "IlFKCHmONATd75UZs806QxswKwpt8l8UN0/hNW1tUcJF5IW1dmJefsb0TELppjft\n"
    "awv/XLb0Brft7jhr+1qJn6WunyQRfEsf5kkoZlHs5Fs9wgB8uKFjvwWY2kg2HFXT\n"
    "mmkWP6j9JM9fg2VdI9yjrZYcYvNWIIVSu57VKQdwlpZtZww1Tkq8mATxdGwIyhgh\n"
    "fDKQXkYuNs474553LBgOhgObJ4Oi7Aeij7XFXfBvTFLJ3ivL9pVYFxg5lUl86pVq\n"
    "5RXSJhiY+gUQFXKOWoqsqmj//////////wIBAg==\n"
    "-----END DH PARAMETERS-----";

static const char g_dh3072_sz[] =
    "-----BEGIN DH PARAMETERS-----\n"
    "MIIBiAKCAYEA///////////JD9qiIWjCNMTGYouA3BzRKQJOCIpnzHQCC76mOxOb\n"
    "IlFKCHmONATd75UZs806QxswKwpt8l8UN0/hNW1tUcJF5IW1dmJefsb0TELppjft\n"
    "awv/XLb0Brft7jhr+1qJn6WunyQRfEsf5kkoZlHs5Fs9wgB8uKFjvwWY2kg2HFXT\n"
    "mmkWP6j9JM9fg2VdI9yjrZYcYvNWIIVSu57VKQdwlpZtZww1Tkq8mATxdGwIyhgh\n"
    "fDKQXkYuNs474553LBgOhgObJ4Oi7Aeij7XFXfBvTFLJ3ivL9pVYFxg5lUl86pVq\n"
    "5RXSJhiY+gUQFXKOWoqqxC2tMxcNBFB6M6hVIavfHLpk7PuFBFjb7wqK6nFXXQYM\n"
    "fbOXD4Wm4eTHq/WujNsJM9cejJTgSiVhnc7j0iYa0u5r8S/6BtmKCGTYdgJzPshq\n"
    "ZFIfKxgXeyAMu+EXV3phXWx3CYjAutlG4gjiT6B05asxQ9tb/OD9EI5LgtEgqTrS\n"
    "yv//////////AgEC\n"
    "-----END DH PARAMETERS-----";

static const char g_dh4096_sz[] =
    "-----BEGIN DH PARAMETERS-----\n"
    "MIICCAKCAgEA///////////JD9qiIWjCNMTGYouA3BzRKQJOCIpnzHQCC76mOxOb\n"
    "IlFKCHmONATd75UZs806QxswKwpt8l8UN0/hNW1tUcJF5IW1dmJefsb0TELppjft\n"
    "awv/XLb0Brft7jhr+1qJn6WunyQRfEsf5kkoZlHs5Fs9wgB8uKFjvwWY2kg2HFXT\n"
    "mmkWP6j9JM9fg2VdI9yjrZYcYvNWIIVSu57VKQdwlpZtZww1Tkq8mATxdGwIyhgh\n"
    "fDKQXkYuNs474553LBgOhgObJ4Oi7Aeij7XFXfBvTFLJ3ivL9pVYFxg5lUl86pVq\n"
    "5RXSJhiY+gUQFXKOWoqqxC2tMxcNBFB6M6hVIavfHLpk7PuFBFjb7wqK6nFXXQYM\n"
    "fbOXD4Wm4eTHq/WujNsJM9cejJTgSiVhnc7j0iYa0u5r8S/6BtmKCGTYdgJzPshq\n"
    "ZFIfKxgXeyAMu+EXV3phXWx3CYjAutlG4gjiT6B05asxQ9tb/OD9EI5LgtEgqSEI\n"
    "ARpyPBKnh+bXiHGaEL26WyaZwycYavTiPBqUaDS2FQvaJYPpyirUTOjbu8LbBN6O\n"
    "+S6O/BQfvsqmKHxZR05rwF2ZspZPoJDDoiM7oYZRW+ftH2EpcM7i16+4G912IXBI\n"
    "HNAGkSfVsFqpk7TqmI2P3cGG/7fckKbAj030Nck0BjGZ//////////8CAQI=\n"
    "-----END DH PARAMETERS-----";
```

## 密钥协商协议

密钥协商协议是在两个对等体之间同意共享秘密的过程。所以，例如，如果Alice和Bob想要通信，则Alice可以使用适当的密钥协商函数，例如Diffie-Hellman(DH)或者Eliptic Curve(椭圆曲线)Diffie-Hellman(ECDH)，使用她的私钥以及Bob的公钥计算出共享秘密。类似地，Bob可以使用他自己的私钥和Alice的公钥计算出相同的共享秘密。然后，该共享秘密可以作为一些对称加密算法的密钥的基础。

以下的代码示例来自于OpenSSL手册，并演示了如何将一个私钥/公钥对(保存在pkey变量中)和某个对等体的公钥(保存在peerkey变量中)组合以导出共享秘密(保存在skey变量中，其长度保存在skeylen中)。显然，同样的代码在对等端执行也会得到同样的共享秘密。

```c
#include <openssl/evp.h>
#include <openssl/rsa.h>

EVP_PKEY_CTX *ctx;
unsigned char *skey;
size_t skeylen;
EVP_PKEY *pkey, *peerkey;

/* NB: 假设pkey, peerkey已经设置完毕 */
	
ctx = EVP_PKEY_CTX_new(pkey);
if (!ctx)
	/* 发生错误 */
if (EVP_PKEY_derive_init(ctx) <= 0)
	/* 错误 */
if (EVP_PKEY_derive_set_peer(ctx, peerkey) <= 0)
	/* 错误 */

/* 确定缓冲区的长度 */
if (EVP_PKEY_derive(ctx, NULL, &skeylen) <= 0)
	/* 错误 */

skey = OPENSSL_malloc(skeylen);

if (!skey)
	/* malloc失败 */
 
if (EVP_PKEY_derive(ctx, skey, &skeylen) <= 0)
	/* 错误 */

/* 共享秘密有skey字节，写入到缓冲区skey中 */
```

你只能使用支持密钥协商协议的EVP_PKEY类型(当前只有DH和ECDH)。显然，在上面的示例代码中一旦不再需要共享秘密时，需要用`OPENSSL_free`进行“释放”。

密钥协商函数的OpenSSL文档可以参见：手册：`EVP_PKEY_derive(3)`。在椭圆曲线 Diffie Hellman页面，还有一个API的使用示例。

注意，以这种方式导出的共享秘密可能不会均匀分布在密钥空间中。由于这种原因，共享秘密通常通过一些进一步的函数来传递，例如像SHA2的消息摘要函数(可能首先将共享秘密和其他定义明确的数据进行组合)。直接使用共享秘密作为加密密钥可能会导致加密实现出现偏差，反过来这可能导致安全弱点。在实践中参考RFC 2631中的2.1.2节的一个例子。

## 消息摘要

消息摘要或者哈希函数以任意消息(任意内容或者任何长度)作为输入，然后产生一个固定长度大小的哈希值作为结果输出。具体来说，该函数具有以下特性：

* 对于任意给定的消息，生成哈希值很简单
* 从任意给定的哈希值去计算出一条消息是不可行的(即函数是单向的)
* 修改消息而不修改哈希值是不可行的
* 找到两条具有相同哈希值的消息是不可行的

OpenSSL库支持许多不同的散列函数包括现在流行的类型：SHA-2系列散列函数(即SHA-224，SHA-256，SHA-384和SHA-512)。

### 哈希函数的使用示例

使用OpenSSL的消息摘要/哈希函数，包括以下几步：

* 创建消息摘要上下文
* 确认要使用的算法(内置的算法在evp.h中被定义)来初始化上下文
* 给出需要计算摘要的消息。如果必要，消息可以分成若干部分，并提供给一些库调用
* 计算摘要
* 如果不再需要，清除上下文

使用`EVP_MD`对象来确定消息摘要算法。这些都内置在库里并且可以通过适当的库调用得到(例如`EVP_sha256()`或者`EVP_sha512()`)。

```c
void digest_message(const unsigned char *message, size_t message_len, unsigned char **digest, unsigned int *digest_len)
{
	EVP_MD_CTX *mdctx;

	if((mdctx = EVP_MD_CTX_create()) == NULL)
		handleErrors();

	if(1 != EVP_DigestInit_ex(mdctx, EVP_sha256(), NULL))
		handleErrors();

	if(1 != EVP_DigestUpdate(mdctx, message, message_len))
		handleErrors();

	if((*digest = (unsigned char *)OPENSSL_malloc(EVP_MD_size(EVP_sha256()))) == NULL)
		handleErrors();

	if(1 != EVP_DigestFinal_ex(mdctx, *digest, digest_len))
		handleErrors();

	EVP_MD_CTX_destroy(mdctx);
}
```

请参考OpenSSL的手册页获取进一步的细节，手册：EVP_DigestInit(3)

## 签名和验证

有两个可用的API接口来进行签名和验证操作。第一个是旧的`EVP_Sign*`和`EVP_Verify*`函数，第二个是较新的，更加灵活的`EVP_DigestSign*`和`EVP_DigestVerify*`函数。尽管这两个接口是类似的，但是新的应用程序应当使用`EVP_DigestSign*`和`EVP_DigestVerify*`函数。

下面的例子使用了新的`EVP_DigestSign*`和`EVP_DigestVerify*`函数来演示签名和验证。第一个例子使用了HMAC，第二个例子使用了RSA密钥对。另外，两个例子的代码可供下载。

注:DSA处理了OpenSSL 1.1.0 中对SSL/TLS加密算法的修改。详情见邮件列表上的DSA与OpenSSL-1.1。

>内容

>1 概述

>2 HMAC

>>2.1 签名

>>2.2 验证

>3 公钥

>>3.1 签名

>>3.2 验证

>4 下载

>5 参见

### 概述

一般来说，对消息进行签名需要三个步骤：

* 用信息摘要/哈希函数和EVP_PKEY密钥对上下文进行初始化
* 添加消息数据(根据需要这个步骤可以重复多次)
* 完成上下文来创建签名

为了初始化，首先你需要选择一种消息摘要算法(参考算法和模式的使用)。其次，你需要提供一个`EVP_PKEY`，它需要包含支持签名的算法的密钥(参考`EVP_PKEYs`的使用))。摘要和密钥都需要提供给`EVP_DigestSignInit`。

调用`EVP_DigestSignUpdate`一次或者多次来添加消息数据。

调用EVP_DigestSignFinal来完成操作，获取签名。

一般来说，验证也遵循相同的步骤。关键的区别在完成这一步：

* 用消息摘要/哈希函数和EVP_PKEY密钥对上下文进行初始化
* 添加消息数据(根据需要这个步骤可以重复多次)
* 用先前的签名完成上下文来验证消息

在完成验证的过程中，在调用中会添加签名。然后EVP_DigestVerifyFinal对消息进行签名验证。

### HMAC

第一个示例会演示如何使用HMAC通过`EVP_DigestSignInit`,`EVP_DigestSignUpdate`还有EVP_DigestSignFinal来对一条消息创建签名。第二部分演示了如何用相同的`EVP_DigestSign`函数来对消息进行签名验证。不要使用`EVP_DigestVerify`函数进行验证。

注意：不要使用`EVP_DigestVerify`函数去验证HMAC。`EVP_DigestVerifyInit`会失败并且产生0x608f096错误：错误：0608F096：数字信封程序：`EVP_PKEY_verify_init`：操作不支持该种密钥类型。

#### 签名 

下面的代码用HMAC对一个字符串进行签名。
	
```c
int sign_it(const byte* msg, size_t mlen, byte** sig, size_t* slen, EVP_PKEY* pkey)
{
	/* 返回到调用者 */
    int result = -1;
    
    if(!msg || !mlen || !sig || !pkey) {
        assert(0);
        return -1;
    }
    
    if(*sig)
        OPENSSL_free(*sig);
    
    *sig = NULL;
    *slen = 0;
    
    EVP_MD_CTX* ctx = NULL;
    
    do
    {
        ctx = EVP_MD_CTX_create();
        assert(ctx != NULL);
        if(ctx == NULL) {
            printf("EVP_MD_CTX_create failed, error 0x%lx\n", ERR_get_error());
            break;
			/* 失败 */
        }
        
        const EVP_MD* md = EVP_get_digestbyname("SHA256");
        assert(md != NULL);
        if(md == NULL) {
            printf("EVP_get_digestbyname failed, error 0x%lx\n", ERR_get_error());
            break;
			/* 失败 */
        }
        
        int rc = EVP_DigestInit_ex(ctx, md, NULL);
        assert(rc == 1);
        if(rc != 1) {
            printf("EVP_DigestInit_ex failed, error 0x%lx\n", ERR_get_error());
            break; 
			/* 失败 */
        }
        
        rc = EVP_DigestSignInit(ctx, NULL, md, NULL, pkey);
        assert(rc == 1);
        if(rc != 1) {
            printf("EVP_DigestSignInit failed, error 0x%lx\n", ERR_get_error());
            break;
			/* 失败 */
        }
        
        rc = EVP_DigestSignUpdate(ctx, msg, mlen);
        assert(rc == 1);
        if(rc != 1) {
            printf("EVP_DigestSignUpdate failed, error 0x%lx\n", ERR_get_error());
            break; 
			/* 失败 */
        }
        
        size_t req = 0;
        rc = EVP_DigestSignFinal(ctx, NULL, &req);
        assert(rc == 1);
        if(rc != 1) {
            printf("EVP_DigestSignFinal failed (1), error 0x%lx\n", ERR_get_error());
            break; 
			/* 失败 */
        }
        
        assert(req > 0);
        if(!(req > 0)) {
            printf("EVP_DigestSignFinal failed (2), error 0x%lx\n", ERR_get_error());
            break;
			/* 失败 */
        }
        
        *sig = OPENSSL_malloc(req);
        assert(*sig != NULL);
        if(*sig == NULL) {
            printf("OPENSSL_malloc failed, error 0x%lx\n", ERR_get_error());
            break;
			/* 失败 */
        }
        
        *slen = req;
        rc = EVP_DigestSignFinal(ctx, *sig, slen);
        assert(rc == 1);
        if(rc != 1) {
            printf("EVP_DigestSignFinal failed (3), return code %d, error 0x%lx\n", rc, 	ERR_get_error());
            break; 
			/* 失败 */
        }
        
        assert(req == *slen);
        if(rc != 1) {
            printf("EVP_DigestSignFinal failed, 	mismatched signature sizes %ld, %ld", 	req, *slen);
            break; 
			/* 失败 */
        }
        
        result = 0;
        
    } while(0);
    
    if(ctx) {
        EVP_MD_CTX_destroy(ctx);
        ctx = NULL;
    }
    
	/* 转换成0/1的结果 */
    return !!result;
}
```

#### 验证

下面的代码用HMAC对一个字符串进行验证。

```c
int verify_it(const byte* msg, size_t mlen, const byte* sig, size_t slen, EVP_PKEY* pkey)
{
	/* 返回到调用者 */
    int result = -1;
    
    if(!msg || !mlen || !sig || !slen || !pkey) {
        assert(0);
        return -1;
    }

    EVP_MD_CTX* ctx = NULL;
    
    do
    {
        ctx = EVP_MD_CTX_create();
        assert(ctx != NULL);
        if(ctx == NULL) {
            printf("EVP_MD_CTX_create failed, error 0x%lx\n", ERR_get_error());
            break;
			/* 失败 */
        }
        
        const EVP_MD* md = EVP_get_digestbyname("SHA256");
        assert(md != NULL);
        if(md == NULL) {
            printf("EVP_get_digestbyname failed, error 0x%lx\n", ERR_get_error());
            break;
			/* 失败 */
        }
        
        int rc = EVP_DigestInit_ex(ctx, md, NULL);
        assert(rc == 1);
        if(rc != 1) {
            printf("EVP_DigestInit_ex failed, error 0x%lx\n", ERR_get_error());
            break; 
			/* 失败 */
        }
        
        rc = EVP_DigestSignInit(ctx, NULL, md, NULL, pkey);
        assert(rc == 1);
        if(rc != 1) {
            printf("EVP_DigestSignInit failed, error 0x%lx\n", ERR_get_error());
            break; 
			/* 失败 */
        }
        
        rc = EVP_DigestSignUpdate(ctx, msg, mlen);
        assert(rc == 1);
        if(rc != 1) {
            printf("EVP_DigestSignUpdate failed, error 0x%lx\n", ERR_get_error());
            break; 
			/* 失败 */
        }
        
        byte buff[EVP_MAX_MD_SIZE];
        size_t size = sizeof(buff);
        
        rc = EVP_DigestSignFinal(ctx, buff, &size);
        assert(rc == 1);
        if(rc != 1) {
            printf("EVP_DigestVerifyFinal failed, error 0x%lx\n", ERR_get_error());
            break; 
			/* 失败 */
        }
        
        assert(size > 0);
        if(!(size > 0)) {
            printf("EVP_DigestSignFinal failed (2)\n");
            break; 
			/* 失败 */
        }
        
        const size_t m = (slen < size ? slen : size);
        result = !!CRYPTO_memcmp(sig, buff, m);
        
        OPENSSL_cleanse(buff, sizeof(buff));
        
    } while(0);
    
    if(ctx) {
        EVP_MD_CTX_destroy(ctx);
        ctx = NULL;
    }
    
	/* 转换成0/1的结果 */
    return !!result;
}
```

### 公钥

第二个示例演示了如何使用公钥通过`EVP_DigestSignInit`,`EVP_DigestSignUpdate`还有`EVP_DigestSignFinal`来对一条消息创建签名。第二个示例演示了如何使用`EVP_DigestVerifyInit`,`EVP_DigestVerifyUpdate`还有`EVP_DigestVerifyFinal`来对消息进行签名验证。不像HMAC一样，可以使用`EVP_DigestVerify`函数来进行验证。

#### 签名

```c	
EVP_MD_CTX *mdctx = NULL;
int ret = 0;
 
*sig = NULL;
 
/* 创建消息摘要上下文 */
if(!(mdctx = EVP_MD_CTX_create())) goto err;
 

/* 初始化DigestSign操作 - 在这个例子中，选择SHA-256作为消息摘要函数 */
if(1 != EVP_DigestSignInit(mdctx, NULL, EVP_sha256(), NULL, key)) goto err;
 
/* 调用更新消息 */
if(1 != EVP_DigestSignUpdate(mdctx, msg, strlen(msg))) goto err;
 
/* 完成DigestSign操作 */
/* 首先调用EVP_DigestSignFinal，采用一个为NULL的sig参数来获得签名的长度。返回的长度保存在slen变量中 */
if(1 != EVP_DigestSignFinal(mdctx, NULL, slen)) goto err;
/* 根据slen的大小为签名分配内存 */
if(!(*sig = OPENSSL_malloc(sizeof(unsigned char) * (*slen)))) goto err;
/* 获得签名 */
if(1 != EVP_DigestSignFinal(mdctx, *sig, slen)) goto err;
 
/* 成功 */
ret = 1;
 
err:
if(ret != 1)
{
	/* 进行一些错误处理 */
}
 
/* 清理 */
if(*sig && !ret) OPENSSL_free(*sig);
if(mdctx) EVP_MD_CTX_destroy(mdctx);
```

注：使用非对称算法进行签名和生成MAC码使用的API没有任何区别。在CMAC的例子中，不需要任何消息摘要函数(可以传递一个NULL参数)。使用`EVP_Sign*`函数进行签名和上面的示例非常相似，只是不再支持MAC码。注意，仅有OpenSSL的 1.1.0 版本(尚未发布)支持CMAC。

需要注意的一个问题是，第一次调用`EVP_DigestSignFinal`时会简单的返回所需缓冲区的最大大小。这和生成签名的大小一般不相同(特别是在DSA和ECDSA的情况下)。这意味着当使用签名时，还需要考虑第二次调用返回的长度值(在这个例子中存放在slen变量中)

想获得有关`EVP_DigestSign*`函数和`EVP_Sign*`函数的进一步细节，请分别参考手册：EVP_DigestSignInit(3)和手册：EVP_SignInit(3)

#### 验证

```c
/* 用公钥初始化`密钥` */
if(1 != EVP_DigestVerifyInit(mdctx, NULL, EVP_sha256(), NULL, key)) goto err;


/* 用公钥初始化`密钥` */
if(1 != EVP_DigestVerifyUpdate(mdctx, msg, strlen(msg))) goto err;

if(1 == EVP_DigestVerifyFinal(mdctx, sig, slen))
{
	/* 成功 */
}
else
{
	/* 失败 */
}
```

注意MAC操作不支持验证操作。进行验证的MAC码是通过调用签名操作产生的，然后确认生成的MAC码和提供的MAC码是否完全相同。重要的是，当将所提供的MAC和预期的MAC进行比较时，无论是否匹配，比较都需要在常数时间内完成。不能做到上面这一点会把代码暴露在计时攻击的威胁下，这可以(例如)使攻击者伪造MAC码。要做到这一点，需要使用CRYPTO_memcmp函数，就像下面示例代码演示的那样。**不要**使用memcmp进行测试：
		
```c
if(!(mdctx = EVP_MD_CTX_create())) goto err;

/* 创建缓冲区来存储收到消息的MAC */
if(!(sigtmp = OPENSSL_malloc(sizeof(unsigned char) * EVP_PKEY_size(key)))) goto err;
sigtmplen = EVP_PKEY_size(key);

/* 计算收到消息的MAC */
if(1 != EVP_DigestSignInit(mdctx, NULL, EVP_sha256(), NULL, key)) goto err;
if(1 != EVP_DigestSignUpdate(mdctx, msg, strlen(msg))) goto err;
if(1 != EVP_DigestSignFinal(mdctx, sigtmp, &sigtmplen)) goto err;
 
/* 检查计算出来的MAC和提供的MAC的长度是否相同 */
if(sigtmplen != slen) goto err;


/* sigtmp是计算出来的MAC，sig是提供的MAC。比较两者 */
if(CRYPTO_memcmp(sig, sigtmp, slen))	
	/* 验证失败 */
else
	/* 验证成功 */
```

想获得有关验证函数的进一步信息，请参考手册：EVP_DigestVerifyInit(3)和手册：EVP_VerifyInit(3)。

### 下载

t-hmac.c.tar.gz - 使用HMAC通过`EVP_DigestSign*`和`EVP_DigestVerify*`函数对一个字符串进行签名和验证的示例程序

t-rsa.c.tar.gz - 使用RSA通过`EVP_DigestSign*`和`EVP_DigestVerify*`函数对一个字符串进行签名和验证的示例程序

## 主机名验证

OpenSSL 1.1.0 会提供内置功能(函数)来进行主机名的检查和验证。 Viktor Dukhovni 在2015年1月提供了相关的实现。从那时起，在Master分支就可用了。OpenSSL 1.1.0 方法发布的同时，代码就开始有了广泛的测试。

一个常见错误是由于OpenSSL的用户假设OpenSSL会验证服务器证书中的主机名所导致的。1.0.2之前的版本不会进行主机名的验证。1.0.2及以上版本包含了对于主机名验证的支持，但是仍然要求用户调用几个函数来设置。

关于主机名验证的手册页从 1.0.2 开始已经可用了。同样见于`X509_check_host()`。

### 示例用法

以下来源于主机名验证并且演示了如何使用OpenSSL的内置主机名验证功能。

```c
const char *servername = NULL;
SSL *ssl = NULL;
X509_VERIFY_PARAM *param = NULL;
...

servername = "www.example.com";
ssl = SSL_new(...);
param = SSL_get0_param(ssl);

/* 进行自动主机名检查 */
X509_VERIFY_PARAM_set_hostflags(param, X509_CHECK_FLAG_NO_PARTIAL_WILDCARDS);
X509_VERIFY_PARAM_set1_host(param, servername, 0);

/* 如果需要的话，配置一个非零的回调 */
SSL_set_verify(ssl, SSL_VERIFY_PEER, 0);


/* 
* 建立SSL连接，应当通过和主机名不匹配的测试来自动检查主机名，连接
* 会失败。(除非你指定了一个尽管验证失败也会返回的回调）如果是这样的话，
* SSL_get_verify_status()可以在连接完成后暴露问题。
*/
...
```

通配符支持通过记录`X509_check_host()`的flag标志来配置，最常用的两个是：

* `**X509_CHECK_FLAG_NO_WILDCARDS**`
* `**X509_CHECK_FLAG_NO_PARTIAL_WILDCARDS**`

用所需的主机名填充`X509_VERIFY_PARAMS`，并让OpenSSL代码自动调用 `X509_check_host`。

这使得日后更容易启用DANE TLSA支持，因为对于DANE来说，DANE-EE(3) TLSA记录需要跳过名称检查，因为DNSSEC TLSA记录提供了必需的名称绑定。

此外，在使用`X509_VERIFY_PARAM`方法时，要尽早进行名称检查，并且对于那些不再继续与未经身份验证的对等体握手的应用程序，也应尽早终止。

有一个新的相关的X509错误代码：`**X509_V_ERR_HOSTNAME_MISMATCH**`

### ssl-conservatory和cURL代码

这是原始信息，可能仍然对 1.0.2 版本以下的openssl有效：

ssl-conservatory仓库展示了如何验证主机名。但是，ssl-conservatory代码不能处理通配符证书，因此需要从cURL借用一些代码作为一种替代方法。这个提交展示了如何将通配符匹配代码从cURL移植到ssl-conservatory代码中。

下面是`openssl_hostname_validation.c`的副本，尽管编译它还需要hostcheck.h,hostcheck.c和`openssl_hostname_validation.h`文件。

```c
/* 来源：https://github.com/iSECPartners/ssl-conservatory */
	
/*
版权所有(C)2012，iSEC合作伙伴。
	
在此授予任何获得本软件及相关文档文件(“软件”)副本的人免费许可，以无限制地处理本软件，包括但不限于使用，
复制，修改，合并，发布，分发，再授权，并且/或者出售本软件的副本，并允许获得本软件的人员在符合以下条件
的情况下：
	
上述版权声明和本许可声明应当包含在本软件的所以副本或者重要部分中。

本软件按“原样”提供，不提供任何明示或者暗示的保证，包括但不限于适销性，适用于特定用途和非侵权的保证。任
何情况下，作者或版权所有人不对因软件或软件使用或软件中的其他行为而产生的任何索赔，损害或其他责任(无论是
在合同中还是在侵权或其他方面的诉讼中)负责。
*/

/*
* 用于使用OpenSSL执行基本的主机名验证的帮助函数
* 在尝试使用该代码之前，请阅读“everything-you-wanted-to-know-about-openssl.pdf”。本白皮书介绍了代码的工作原理，如何使用它，以及它的局限性。
* 作者：Alban Diquet
* 许可证：参见LICENSE
*/


// 摆脱OSX10.7以及更大的depreciation警告。
#if defined(__APPLE__) && defined(__clang__)
#pragma clang diagnostic ignored "-Wdeprecated-declarations"
#endif

#include <openssl/x509v3.h>
#include <openssl/ssl.h>

#include "openssl_hostname_validation.h"
#include "hostcheck.h"

#define HOSTNAME_MAX_SIZE 255

/**
尝试在证书的公用名称字段中寻找主机名的匹配项
如果找到匹配项，返回MatchFound；
如果没有找到匹配项，返回MatchNotFound；
如果公用名称中存在嵌入的NULL字符，返回MalformedCertificate；
如果无法提取公用名称，返回Error；
*/
static HostnameValidationResult matches_common_name(const char *hostname, const X509 *server_cert) {
	int common_name_loc = -1;
    X509_NAME_ENTRY *common_name_entry = NULL;
    ASN1_STRING *common_name_asn1 = NULL;
    char *common_name_str = NULL;
	// 在证书的附属字段中查找CN字段的位置
    common_name_loc = X509_NAME_get_index_by_NID(X509_get_subject_name((X509 *) server_cert), NID_commonName, -1);
    if (common_name_loc < 0) {
            return Error;
    }

	// 提取CN字段
    common_name_entry = X509_NAME_get_entry(X509_get_subject_name((X509 *) server_cert), common_name_loc);
    if (common_name_entry == NULL) {
            return Error;
    }

	// 将CN字段转换为C中的字符串
    common_name_asn1 = X509_NAME_ENTRY_get_data(common_name_entry);
    if (common_name_asn1 == NULL) {
            return Error;
    }
    common_name_str = (char *) ASN1_STRING_data(common_name_asn1);

	// 确保CN中没有嵌入的NULL字符
    if ((size_t)ASN1_STRING_length(common_name_asn1) != strlen(common_name_str)) {
            return MalformedCertificate;
    }
	
	// 将预期的主机名与CN进行比较
    if (Curl_cert_hostcheck(common_name_str, hostname) == CURL_HOST_MATCH) {
            return MatchFound;
    }
    else {
            return MatchNotFound;
   	}
}

/**
尝试在证书的附属备用的名称扩展中找到主机名的匹配项
如果找到匹配项，返回MatchFound；
如果没有找到匹配项，返回MatchNotFound；
如果任何主机名中存在嵌入的NULL字符，返回MalformedCertificate；
如果证书中不存在SAN扩展，返回NoSANPresent；
*/
static HostnameValidationResult matches_subject_alternative_name(const char *hostname, const X509 *server_cert) {
    HostnameValidationResult result = MatchNotFound;
    int i;
    int san_names_nb = -1;
    STACK_OF(GENERAL_NAME) *san_names = NULL;

	// 尝试从证书中的SAN扩展中提取出名称
    san_names = X509_get_ext_d2i((X509 *) server_cert, NID_subject_alt_name, NULL, NULL);
    if (san_names == NULL) {
            return NoSANPresent;
	}
    san_names_nb = sk_GENERAL_NAME_num(san_names);

	// 检查扩展中的每个名称
    for (i=0; i<san_names_nb; i++) {
            const GENERAL_NAME *current_name = sk_GENERAL_NAME_value(san_names, i);

            if (current_name->type == GEN_DNS) {
					// 当前名称是DNS名称，对其进行检查
                    char *dns_name = (char *) ASN1_STRING_data(current_name->d.dNSName);

					// 确保DNS名称中没有嵌入的NULL字符
                    if ((size_t)ASN1_STRING_length(current_name->d.dNSName) != strlen(dns_name)) {
                            result = MalformedCertificate;
                            break;
                    }
                    else { 
						    // 将预期的主机名与DNS名称进行比较 
                            if (Curl_cert_hostcheck(dns_name, hostname)
                                    == CURL_HOST_MATCH) {
                                    result = MatchFound;
                                    break;
                            }
                    }
            }
    }
    sk_GENERAL_NAME_pop_free(san_names, GENERAL_NAME_free);

    return result;
}

/**
通过在服务器证书中查询预期的主机名来验证服务器的身份。如RFC 6125中所述，它首先尝试在附属备用的名称扩展中找到匹配项。
如果扩展名不在证书中，将检查公用名称。
如果找到匹配项，返回MatchFound；
如果未找到匹配项，返回MatchNotFound；
如果任何主机名中存在嵌入的NULL字符，返回MalformedCertificate；
如果有错误，返回Error；
*/
HostnameValidationResult validate_hostname(const char *hostname, const X509 *server_cert) {
    HostnameValidationResult result;

    if((hostname == NULL) || (server_cert == NULL))
            return Error;

	// 首先尝试附属备用的名称扩展
    result = matches_subject_alternative_name(hostname, server_cert);
    if (result == NoSANPresent) {
			// 未找到扩展名：尝试公用名称
            result = matches_common_name(hostname, server_cert);
    }

    return result;
}
```
##EVP


EVP函数提供了一个高级的接口给OpenSSL密码函数。

它们提供了以下的特征

* 一个独立的始终如一的接口，不管是不是优先的算法或者模式
* 支持一个广泛范围内的算法
* 加密和解密都使用对称和不对称的算法
* 签署/验证
* 关键推导
* 安全的哈希函数
* 信息验证码
* 支持外部加密引擎

###处理EVP_PKEYs

EVP_PKEY对象被用于存储一个公共的密钥以及（可选择的）一个私密的密钥，以及一些关联的算法和参数。它们也能够存储对称的MAC密钥。

下列的EVP_PKEY类型是支持的

* EVP_PKEY_EC:椭圆曲线密钥（对ECDSA（椭圆曲线密钥算法）和ECDH（椭圆曲线密钥交换体制））支持签名/验证操作，以及密码的推导。
* EVP_PKEY_RSA：RSA支持签名/验证，以及加密解密。
* EVP_PKEY_DH:Diffie Hellman密钥交换体系，用于密钥推导
* EVP_PKEY_DSA:DSA密码用于签名/验证
* EVP_PKEY_HMAC：HMAC密码用于收集信息认证码
* EVP_PKEY_CMAC:CMAC密码用于收集信息验证码

*注意*：DSA处理改变了SSL/TLS密码适合OpenSSL1.1.0. 具体细节请看DSA with OpenSSL-1.1在邮件列表上。参考手册:`EVP_PKEY_new(3)`手册得到更多信息关于创建一个`EVP_PKEY` 对象，以及`Manual:EVP_PKEY_set1_RSA(3)`页面得到关于怎么初始化`EVP_PKEY`的信息。

参考EVP密钥和参数提取`EVP_Key_and_Parameter_Generation)`得到关于提取新密钥以及相关的参数的信息。

###处理算法和模板

密码以及电文破译算法是由一个唯一的`EVP_CIPHER`以及`EVP_MD`对象各自确认的。你并不需要自己创立它们，而是使用内部的函数来得到一个你想使用的算法。有关密码和信息摘要的完整列表，请参阅**evp.h**头文件。从**evp.h**中提取了一些EVP_CIPHER函数，如下所示：


```c
const EVP_CIPHER *EVP_aes_128_ctr(void);
const EVP_CIPHER *EVP_aes_128_ccm(void);
const EVP_CIPHER *EVP_aes_128_gcm(void);
const EVP_CIPHER *EVP_aes_128_xts(void);
const EVP_CIPHER *EVP_aes_192_ecb(void);
const EVP_CIPHER *EVP_aes_192_cbc(void);

这些密码都是AES(高级加密标准)算法的变体。有两种不同长度的密钥显示，分别用于128位密钥和192位密钥。还存在所示的各种不同的加密模式，即CTR，CCM，GCM，XTS，ECB和CBC。不是所有算法都支持所有加密模式，所以你要在**EVP.h**里面寻找你想要的特定组合。以下（编辑的）从**evp.h**的提取显示了一些示例消息摘要函数。


```c
const EVP_MD *EVP_md2(void);
const EVP_MD *EVP_md4(void);
const EVP_MD *EVP_md5(void);
const EVP_MD *EVP_sha1(void);
const EVP_MD *EVP_sha224(void);
const EVP_MD *EVP_sha256(void);
const EVP_MD *EVP_sha384(void);
const EVP_MD *EVP_sha512(void);
```
从这些函数返回的对象是内置的，不需要在使用后“释放”。

###加密操作 
以下加密操作是可能的。 有关详细信息，请参阅相关页面

* 对称加密和解密
* 经过身份的加密和解密
* 包络的不对称加密和解密
* 签名和验证（包括消息验证码）
* 消息摘要
* 主要协议
* 键和参数生成
* 'EVP_Key_and_Parameter_Generation)'


##EVP对称加密和解密
OpenSSL中的libcrypto库提供了在各种算法和模式中执行对称加密和解密操作的函数。本页向您介绍执行简单加密和相应解密操作的基础知识。

为了执行加密/解密，您需要知道：



* 你的算法
* 你的模式
* 你的密钥
* 你的初始化向量（IV）

这个页面假设你知道这些东西是什么意思。 如果不这样，请参阅加密基本知识
###建立
下面的代码建立了程序。在这个例子中，我们将采用一个简单的信息（The quick brown fox jumps over the lazy dog），然后使用预定义的密钥和初始化向量（IV）对它进行加密。在这个例子中，密钥和IV已经被硬编码 - 在真实的情况下，你永远不会这样做！在加密之后，我们将解密所产生的密文。然后（希望）得到我们一开始的信息。该程序需要定义两个函数：“encrypt”和“decrypt”。 我们将在页面的后面进一步定义。


```c
 #include <openssl/conf.h>
 #include <openssl/evp.h>
 #include <openssl/err.h>
 #include <string.h>

 int main (void)
{
/* Set up the key and iv. Do I need to say to not hard code these in a
 * real application? :-)
 */

 /* A 256 bit key */
 unsigned char *key = (unsigned char *)"01234567890123456789012345678901";

 /* A 128 bit IV */
 unsigned char *iv = (unsigned char *)"01234567890123456";

 /* Message to be encrypted */
 unsigned char *plaintext =(unsigned char *)"The quick brown fox jumps over the lazy dog";

 /* Buffer for ciphertext. Ensure the buffer is long enough for the
  * ciphertext which may be longer than the plaintext, dependant on the
  * algorithm and mode
  */
  unsigned char ciphertext[128];

 /* Buffer for the decrypted text */
 unsigned char decryptedtext[128];

 int decryptedtext_len, ciphertext_len;

/* Initialise the library */
ERR_load_crypto_strings();
OpenSSL_add_all_algorithms();
OPENSSL_config(NULL);

/* Encrypt the plaintext */
ciphertext_len = encrypt (plaintext, strlen ((char *)plaintext), key, iv,
                      	      ciphertext);

/* Do something useful with the ciphertext here */
printf("Ciphertext is:\n");
BIO_dump_fp (stdout, (const char *)ciphertext, ciphertext_len);

/* Decrypt the ciphertext */
decryptedtext_len = decrypt(ciphertext, ciphertext_len, key, iv,
decryptedtext);

/* Add a NULL terminator. We are expecting printable text */
decryptedtext[decryptedtext_len] = '\0';

/* Show the decrypted text */
printf("Decrypted text is:\n");
printf("%s\n", decryptedtext);

/* Clean up */
EVP_cleanup();
ERR_free_strings();

return 0;
}
```

这个程序设置了256位的密钥和128位的初始化向量。这适用于我们将在CBC模式下进行的256位AES加密。确保你使用正确的密钥和IV长度为你选择的密码，否则会出错！

我们还设置了缓冲区来放置密文。特别重要的是确保此缓冲区对于预期的密文足够大，否则您可能会看到程序崩溃（或可能在您的代码中引入安全漏洞）。注意：密文可能比明文长（例如，如果正在使用填充）。

我们还需要一个帮助函数来处理任何错误。 这将简单地将任何错误消息从OpenSSL错误堆栈转储到屏幕，然后中止程序。


```c
void handleErrors(void)
{
ERR_print_errors_fp(stderr);
abort();
}
```
###加密信息
我们已经建立了程序，需要定义encrypt函数。这个函数将采用明文，明文的长度，要使用的密钥和IV作为参数。 我们还将用一个缓冲区来放置密文（我们假设它足够长），并且将返回我们已经写入的密文的长度。

加密包括以下阶段

* 建立内容
* 加密操作的初始化
* 提供要加密的明文
* 完成加密

在初始化期间，我们将提供一个`EVP_CIPHER`对象。 在这种情况下，我们使用`EVP_aes_256_cbc（）`，它在CBC模式下使用256位密钥的AES算法。 有关更多详细信息，请参阅“EVP＃使用算法和模式”。


```c
int encrypt(unsigned char *plaintext, int plaintext_len, unsigned char *key,
unsigned char *iv, unsigned char *ciphertext)
{
EVP_CIPHER_CTX *ctx;

int len;

int ciphertext_len;

/* Create and initialise the context */
if(!(ctx = EVP_CIPHER_CTX_new())) handleErrors();

/* Initialise the encryption operation. IMPORTANT - ensure you use a key
* and IV size appropriate for your cipher
* In this example we are using 256 bit AES (i.e. a 256 bit key). The
* IV size for *most* modes is the same as the block size. For AES this
* is 128 bits */
if(1 != EVP_EncryptInit_ex(ctx, EVP_aes_256_cbc(), NULL, key, iv))
handleErrors();

/* Provide the message to be encrypted, and obtain the encrypted output.
* EVP_EncryptUpdate can be called multiple times if necessary
*/
if(1 != EVP_EncryptUpdate(ctx, ciphertext, &len, plaintext, plaintext_len))
handleErrors();
ciphertext_len = len;

/* Finalise the encryption. Further ciphertext bytes may be written at
* this stage.
*/
if(1 != EVP_EncryptFinal_ex(ctx, ciphertext + len, &len)) handleErrors();
ciphertext_len += len;

/* Clean up */
EVP_CIPHER_CTX_free(ctx);

return ciphertext_len;
}
```

###解密信息

最后，我们需要定义“decrypt”操作。 这与加密非常相似，包括以下阶段：

* 建立内容
* 初始化解密操作
* 提供要解密的密文
* 完成解密

再次通过参数，我们将接收要解密的密文，密文的长度，密钥和IV。 我们还将收到一个缓冲区，将解密的文本放入，并返回我们发现的明文的长度。

注意，我们已经得到了密文的长度。 这是必需的，因为你不能使用诸如“strlen”这样的数据 - 它是二进制！ 同样，即使在这个例子中我们的纯文本是ASCII文本，OpenSSL也不知道。 尽管名称明文可以是二进制数据，因此没有NULL终止符将放在末尾（除非你加密NULL当然）

解密函数


```c
int decrypt(unsigned char *ciphertext, int ciphertext_len, unsigned char *key,
unsigned char *iv, unsigned char *plaintext)
{
EVP_CIPHER_CTX *ctx;

int len;

int plaintext_len;

/* Create and initialise the context */
if(!(ctx = EVP_CIPHER_CTX_new())) handleErrors();

/* Initialise the decryption operation. IMPORTANT - ensure you use a key
* and IV size appropriate for your cipher
* In this example we are using 256 bit AES (i.e. a 256 bit key). The
* IV size for *most* modes is the same as the block size. For AES this
* is 128 bits */
if(1 != EVP_DecryptInit_ex(ctx, EVP_aes_256_cbc(), NULL, key, iv))
handleErrors();

/* Provide the message to be decrypted, and obtain the plaintext output.
* EVP_DecryptUpdate can be called multiple times if necessary
*/
if(1 != EVP_DecryptUpdate(ctx, plaintext, &len, ciphertext, ciphertext_len))
handleErrors();
plaintext_len = len;

/* Finalise the decryption. Further plaintext bytes may be written at
* this stage.
*/
if(1 != EVP_DecryptFinal_ex(ctx, plaintext + len, &len)) handleErrors();
plaintext_len += len;

/* Clean up */
EVP_CIPHER_CTX_free(ctx);

return plaintext_len;
}
```
###密文输出

如果一切顺利，你应该得到如下输出：
```c
Ciphertext is:
0000 - e0 6f 63 a7 11 e8 b7 aa-9f 94 40 10 7d 46 80 a1   .oc.......@.}F..
0010 - 17 99 43 80 ea 31 d2 a2-99 b9 53 02 d4 39 b9 70   ..C..1....S..9.p
0020 - 2c 8e 65 a9 92 36 ec 92-07 04 91 5c f1 a9 8a 44   ,.e..6.....\...D
Decrypted text is:
The quick brown fox jumps over the lazy dog
```

有关对称加密和解密操作的更多详细信息，请参阅OpenSSL文档手册：`EVP_EncryptInit（3）`

##填充
默认情况下，OpenSSL使用PKCS填充。 如果您使用的模式允许您更改填充，则可以使用`EVP_CIPHER_CTX_set_padding`更改它。手册页`EVP_CIPHER_CTX_set_padding（）`启用或禁用填充。 默认情况下，使用标准块填充填充加密操作，并在解密时检查和删除填充。 如果填充参数为零，则不执行填充，则加密或解密的数据总量必须是块大小的倍数，否则将发生错误...

PKCS填充通过添加值为n的n个填充字节来使加密数据的总长度成为块大小的倍数。 填充总是添加，所以如果数据已经是块大小的倍数，n将等于块大小。 例如，如果块大小是8并且11字节将被加密，则将添加值5的5个填充字节...

如果禁用填充，则解密操作将仅在解密的数据的总量是块大小的倍数时才会成功....
###C++程序

有时有如何使用来自C ++程序的EVP接口的问题。 一般来说，使用来自C ++程序的EVP接口与从C程序使用它们相同。

您可以下载一个使用EVP对称加密和C ++ 11称为`evp_encrypt.cxx`的示例程序。 该示例使用自定义分配器来归零内存，C ++智能指针来管理资源，并使用`basic_string`和自定义分配器提供了一个`secure_string`。 你需要使用g ++ -std = c ++ 11 ...来编译它，因为`std :: unique_ptr`。

您还应确保使用-fexception配置构建，以确保C++异常按预期通过C代码传递。 你应该避免其他标志，如-fno-exceptions和-fno-rtti。

程序主要使用AES-256在CBC模式下简单地加密和解密字符串：


```c
typedef unsigned char byte;
typedef std::basic_string<char, std::char_traits<char>, zallocator<char> > secure_string;
using EVP_CIPHER_CTX_ptr = std::unique_ptr<EVP_CIPHER_CTX, decltype(&::EVP_CIPHER_CTX_free)>;
...

int main(int argc, char* argv[])
{
// Load the necessary cipher
EVP_add_cipher(EVP_aes_256_cbc());

// plaintext, ciphertext, recovered text
secure_string ptext = "Yoda said, Do or do not. There is no try.";
secure_string ctext, rtext;

byte key[KEY_SIZE], iv[BLOCK_SIZE];
gen_params(key, iv);
  
aes_encrypt(key, iv, ptext, ctext);
aes_decrypt(key, iv, ctext, rtext);
    
OPENSSL_cleanse(key, KEY_SIZE);
OPENSSL_cleanse(iv, BLOCK_SIZE);

std::cout << "Original message:\n" << ptext << std::endl;
std::cout << "Recovered message:\n" << rtext << std::endl;

return 0;
}
```

加密程序如下。 解密程序类似：


```c
void aes_encrypt(const byte key[KEY_SIZE], const byte iv[BLOCK_SIZE], const secure_string& ptext, secure_string& ctext)
{
EVP_CIPHER_CTX_ptr ctx(EVP_CIPHER_CTX_new(), ::EVP_CIPHER_CTX_free);
int rc = EVP_EncryptInit_ex(ctx.get(), EVP_aes_256_cbc(), NULL, key, iv);
if (rc != 1)
throw std::runtime_error("EVP_EncryptInit_ex failed");

// Cipher text expands upto BLOCK_SIZE
ctext.resize(ptext.size()+BLOCK_SIZE);
int out_len1 = (int)ctext.size();

rc = EVP_EncryptUpdate(ctx.get(), (byte*)&ctext[0], &out_len1, (const byte*)&ptext[0], (int)ptext.size());
if (rc != 1)
throw std::runtime_error("EVP_EncryptUpdate failed");
  
int out_len2 = (int)ctext.size() - out_len1;
rc = EVP_EncryptFinal_ex(ctx.get(), (byte*)&ctext[0]+out_len1, &out_len2);
if (rc != 1)
throw std::runtime_error("EVP_EncryptFinal_ex failed");

// Set cipher text size now that we know it
ctext.resize(out_len1 + out_len2);
}
```

###注意一些不寻常的模式
这里值得一提的是XTS模式（例如`EVP_aes_256_xts（）`）。 除了在IV参数中提供“tweak”之外，它的工作方式与上面所示的完全相同。 另一个“困扰”是XTS模式期望的密钥是正常的两倍。 因此，`EVP_aes_256_xts（）`期望一个512位长的密钥。
认证加密模式（GCM）的工作方式与上述方式基本相同，但需要一些特殊处理。 有关更多详细信息，请参阅“EVP身份验证加密和解密”。
##EVP认证加密和解密
EVP接口支持执行验证加密和解密的能力，以及将未加密的关联数据附加到消息的选项。 这种带有关联数据（AEAD）方案的认证加密通过加密数据来提供机密性，并且还通过在加密数据上创建MAC标签来提供真实性保证。 MAC标签将确保数据不会在传输和存储期间意外更改或恶意篡改。

有多种AEAD操作模式。 模式包括EAX，CCM和GCM模式。 使用AEAD模式几乎等同于使用诸如CBC，CFB和OFB模式的标准对称加密模式。

与标准对称加密一样，您需要了解以下内容：

* 算法（现在只支持AES）
* 模式（现在只支持CCM和GCM）
* 密钥
* 初始化向量（IV）

此外，您可以（可选）提供一些额外的已验证数据（AAD）。 AAD数据未加密，并且通常以明文连同密文一起传递给接收者。 AAD的示例是IP报头中与IPsec一起使用的IP地址和端口号。

加密操作的输出将是密文和标签。 随后在解密操作期间使用标签以确保密文和AAD未被篡改。

OpenSSL手册介绍了GCM和CCM模式的用法：`Manual：EVP_EncryptInit（3）#GCM_Mode`。

###使用GCM模式的已验证加密

以与在此所述的对称加密大致相同的方式执行加密。 主要区别是：

* 您可以选择使用EVP_CIPHER_CTX_ctrl传递IV长度
* AAD数据在对EVP_EncryptUpdate的零个或多个调用中传递，输出缓冲区设置为NULL
* 在EVP_EncryptFinal_ex调用之后，对EVP_CIPHER_CTX_ctrl的新调用将检索标记

请参考下面的代码示例：


```c
int encrypt(unsigned char *plaintext, int plaintext_len, unsigned char *aad,
int aad_len, unsigned char *key, unsigned char *iv,
unsigned char *ciphertext, unsigned char *tag)
{
EVP_CIPHER_CTX *ctx;

int len;

int ciphertext_len;


/* Create and initialise the context */
if(!(ctx = EVP_CIPHER_CTX_new())) handleErrors();

/* Initialise the encryption operation. */
if(1 != EVP_EncryptInit_ex(ctx, EVP_aes_256_gcm(), NULL, NULL, NULL))
handleErrors();

/* Set IV length if default 12 bytes (96 bits) is not appropriate */
if(1 != EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_GCM_SET_IVLEN, 16, NULL))
handleErrors();

/* Initialise key and IV */
if(1 != EVP_EncryptInit_ex(ctx, NULL, NULL, key, iv)) handleErrors();

/* Provide any AAD data. This can be called zero or more times as
 * required
 */
if(1 != EVP_EncryptUpdate(ctx, NULL, &len, aad, aad_len))
handleErrors();

/* Provide the message to be encrypted, and obtain the encrypted output.
 * EVP_EncryptUpdate can be called multiple times if necessary
 */
if(1 != EVP_EncryptUpdate(ctx, ciphertext, &len, plaintext, plaintext_len))
handleErrors();
ciphertext_len = len;

/* Finalise the encryption. Normally ciphertext bytes may be written at
 * this stage, but this does not occur in GCM mode
 */
if(1 != EVP_EncryptFinal_ex(ctx, ciphertext + len, &len)) handleErrors();
ciphertext_len += len;

/* Get the tag */
if(1 != EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_GCM_GET_TAG, 16, tag))
handleErrors();
	
/* Clean up */
EVP_CIPHER_CTX_free(ctx);
	
return ciphertext_len;
}
```
###使用GCM模式的已验证解密

同样，解密操作与如这里所描述的正常对称解密大致相同。 主要区别是：

* 您可以选择使用`EVP_CIPHER_CTX_ctrl`传递IV长度
* AAD数据在对`EVP_DecryptUpdate`的零个或多个调用中传递，输出缓冲区设置为NULL
* 在`EVP_DecryptFinal_ex`调用之前，对`EVP_CIPHER_CTX_ctrl`的新调用提供了标记
*` EVP_DecryptFinal_ex`的非正值返回值应被视为认证密文和/或AAD的失败。 它不一定表示更严重的错误。

参考下面代码示例


```c
int decrypt(unsigned char *ciphertext, int ciphertext_len, unsigned char *aad,int aad_len, unsigned char *tag, unsigned char *key, unsigned char *iv,
unsigned char *plaintext)
{
EVP_CIPHER_CTX *ctx;
int len;
int plaintext_len;
int ret;

/* Create and initialise the context */
if(!(ctx = EVP_CIPHER_CTX_new())) handleErrors();

/* Initialise the decryption operation. */
if(!EVP_DecryptInit_ex(ctx, EVP_aes_256_gcm(), NULL, NULL, NULL))
handleErrors();

/* Set IV length. Not necessary if this is 12 bytes (96 bits) */
if(!EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_GCM_SET_IVLEN, 16, NULL))
handleErrors();

/* Initialise key and IV */
if(!EVP_DecryptInit_ex(ctx, NULL, NULL, key, iv)) handleErrors();

/* Provide any AAD data. This can be called zero or more times as
* required
*/
if(!EVP_DecryptUpdate(ctx, NULL, &len, aad, aad_len))
handleErrors();

/* Provide the message to be decrypted, and obtain the plaintext output.
* EVP_DecryptUpdate can be called multiple times if necessary
*/
if(!EVP_DecryptUpdate(ctx, plaintext, &len, ciphertext, ciphertext_len))
handleErrors();
plaintext_len = len;
	
/* Set expected tag value. Works in OpenSSL 1.0.1d and later */
if(!EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_GCM_SET_TAG, 16, tag))
handleErrors();

/* Finalise the decryption. A positive return value indicates success,
 * anything else is a failure - the plaintext is not trustworthy.
 */
ret = EVP_DecryptFinal_ex(ctx, plaintext + len, &len);

/* Clean up */
EVP_CIPHER_CTX_free(ctx);

if(ret > 0)
{
/* Success */
plaintext_len += len;
return plaintext_len;
}
else
{
/* Verify failed */
return -1;
}
}
```

###使用CCM模式的已验证加密

使用CCM模式加密与使用GCM加密非常相似，但需要记住一些其他事项。

* 您只能对AAD调用EVP_EncryptUpdate一次，对纯文本调用一次。
* 总明文长度必须传递给EVP_EncryptUpdate（仅当传递AAD时才需要）
* 可选地，标签和IV长度也可以被传递。 如果不是，则使用默认值（AES标记为12字节，AES IV为7字节）

示例代码：


```c
int encryptccm(unsigned char *plaintext, int plaintext_len, unsigned char *aad,int aad_len, unsigned char *key, unsigned char *iv,
unsigned char *ciphertext, unsigned char *tag)
{
EVP_CIPHER_CTX *ctx;

int len;

int ciphertext_len;


/* Create and initialise the context */
if(!(ctx = EVP_CIPHER_CTX_new())) handleErrors();

/* Initialise the encryption operation. */
if(1 != EVP_EncryptInit_ex(ctx, EVP_aes_256_ccm(), NULL, NULL, NULL))
handleErrors();

/* Setting IV len to 7. Not strictly necessary as this is the default
 * but shown here for the purposes of this example */
if(1 != EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_CCM_SET_IVLEN, 7, NULL))
handleErrors();

/* Set tag length */
EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_CCM_SET_TAG, 14, NULL);

/* Initialise key and IV */
if(1 != EVP_EncryptInit_ex(ctx, NULL, NULL, key, iv)) handleErrors();

/* Provide the total plaintext length
 */
if(1 != EVP_EncryptUpdate(ctx, NULL, &len, NULL, plaintext_len))
handleErrors();
	
/* Provide any AAD data. This can be called zero or one times as
* required
*/
if(1 != EVP_EncryptUpdate(ctx, NULL, &len, aad, aad_len))
handleErrors();

/* Provide the message to be encrypted, and obtain the encrypted output.
* EVP_EncryptUpdate can only be called once for this
*/
if(1 != EVP_EncryptUpdate(ctx, ciphertext, &len, plaintext, 	plaintext_len))
handleErrors();
ciphertext_len = len;

/* Finalise the encryption. Normally ciphertext bytes may be written at
 * this stage, but this does not occur in CCM mode
 */
if(1 != EVP_EncryptFinal_ex(ctx, ciphertext + len, &len)) handleErrors();
ciphertext_len += len;

/* Get the tag */
if(1 != EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_CCM_GET_TAG, 14, tag))
handleErrors();

/* Clean up */
EVP_CIPHER_CTX_free(ctx);

return ciphertext_len;
}
```
###使用CCM模式的已认证解密

使用CCM模式的解密与使用GCM的解密非常相同，但需要考虑一些其他事项。

* 您只能对AAD调用EVP_EncryptUpdate一次，对纯文本调用一次。
* 总明文长度必须传递给EVP_EncryptUpdate（仅当传递AAD时才需要）
* 可选地，标签和IV长度也可以被传递。 如果不是，则使用默认值（AES标记为12字节，AES IV为7字节）
* 标签验证在调用最终EVP_ DecryptUpdate时执行，并且由返回值反映：没有调用EVP_DecryptFinal。

示例代码：


```c
int decryptccm(unsigned char *ciphertext, int ciphertext_len, unsigned char *aad,int aad_len, unsigned char *tag, unsigned char *key, unsigned char *iv,
unsigned char *plaintext)
{
EVP_CIPHER_CTX *ctx;
int len;
int plaintext_len;
int ret;

/* Create and initialise the context */
if(!(ctx = EVP_CIPHER_CTX_new())) handleErrors();

/* Initialise the decryption operation. */
if(1 != EVP_DecryptInit_ex(ctx, EVP_aes_256_ccm(), NULL, NULL, NULL))
handleErrors();

/* Setting IV len to 7. Not strictly necessary as this is the default
* but shown here for the purposes of this example */
if(1 != EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_CCM_SET_IVLEN, 7, NULL))
handleErrors();

/* Set expected tag value. */
if(1 != EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_CCM_SET_TAG, 14, tag))
handleErrors();

/* Initialise key and IV */
if(1 != EVP_DecryptInit_ex(ctx, NULL, NULL, key, iv)) handleErrors();


/* Provide the total ciphertext length
 */
if(1 != EVP_DecryptUpdate(ctx, NULL, &len, NULL, ciphertext_len))
handleErrors();

/* Provide any AAD data. This can be called zero or more times as
 * required
 */
if(1 != EVP_DecryptUpdate(ctx, NULL, &len, aad, aad_len))
handleErrors();

/* Provide the message to be decrypted, and obtain the plaintext output.
 * EVP_DecryptUpdate can be called multiple times if necessary
 */
ret = EVP_DecryptUpdate(ctx, plaintext, &len, ciphertext, 	ciphertext_len);

plaintext_len = len;

/* Clean up */
EVP_CIPHER_CTX_free(ctx);
	
if(ret > 0)
{
/* Success */
return plaintext_len;
}
else
{
/* Verify failed */
return -1;
}
}
```
###AES / GCM中的潜在问题
当认证标记大小不是块大小的倍数时，认证加密接口的早期版本需要使用0大小的数组（而不是NULL数组）来获得正确的认证标记（例如，认证标记大小为20 字节）。 有关问题和解决方法的更多信息，请参阅Issue #2859: Possible bug in AES GCM mode and Possible bug in GCM/GMAC with (just) AAD of size unequal to block size


##EVP不对称加密和解密

使用非对称密钥的加密和解密在计算上是昂贵的。 通常，消息不是使用这样的密钥直接加密，而是使用对称的“会话”密钥加密。 然后，该密钥本身随后使用公钥加密。 在OpenSSL中，此组合称为包络。 还可以用多个公钥加密会话密钥。 这样，消息可以被发送到多个不同的接收者（对于每个使用的公共密钥一个）。 会话密钥对于每个收件人是相同的。

用于处理包络的OpenSSL手册页可以在这里找到：Manual:EVP_SealInit(3)和Manual:EVP_OpenInit(3)
###密封包络

包络使用EVP_Seal *函数集密封，操作由以下步骤组成：

* 初始化上下文
* 初始化密封操作，提供将使用的对称密码，以及用于加密会话密钥的公钥集合
* 提供要加密的消息
* 完成加密操作

示例代码：


```c
int envelope_seal(EVP_PKEY **pub_key, unsigned char *plaintext, int plaintext_len,unsigned char **encrypted_key, int *encrypted_key_len, unsigned char *iv,unsigned char *ciphertext)
{
EVP_CIPHER_CTX *ctx;

int ciphertext_len;

int len;


/* Create and initialise the context */
if(!(ctx = EVP_CIPHER_CTX_new())) handleErrors();

/* Initialise the envelope seal operation. This operation generates
 * a key for the provided cipher, and then encrypts that key a number
 * of times (one for each public key provided in the pub_key array). In
 * this example the array size is just one. This operation also
 * generates an IV and places it in iv. */
if(1 != EVP_SealInit(ctx, EVP_aes_256_cbc(), encrypted_key,
encrypted_key_len, iv, pub_key, 1))
handleErrors();

/* Provide the message to be encrypted, and obtain the encrypted output.
* EVP_SealUpdate can be called multiple times if necessary
 */
if(1 != EVP_SealUpdate(ctx, ciphertext, &len, plaintext, plaintext_len))
handleErrors();
ciphertext_len = len;

/* Finalise the encryption. Further ciphertext bytes may be written at
 * this stage.
 */
if(1 != EVP_SealFinal(ctx, ciphertext + len, &len)) handleErrors();
ciphertext_len += len;

/* Clean up */
EVP_CIPHER_CTX_free(ctx);

return ciphertext_len;
}

###打开包络
使用EVP_Open *函数集在以下步骤中打开一个包络：

* 初始化上下文
* 初始化打开操作，提供已使用的对称密码，以及私钥解密会话密钥
* 提供要使用会话密钥解密和解密的消息
* 完成解密操作

示例代码：


```c
int envelope_open(EVP_PKEY *priv_key, unsigned char *ciphertext, int ciphertext_len,unsigned char *encrypted_key, int encrypted_key_len, unsigned char *iv,unsigned char *plaintext)
{
EVP_CIPHER_CTX *ctx;

int len;

int plaintext_len;


/* Create and initialise the context */
if(!(ctx = EVP_CIPHER_CTX_new())) handleErrors();

/* Initialise the decryption operation. The asymmetric private key is
 * provided and priv_key, whilst the encrypted session key is held in
 * encrypted_key */
if(1 != EVP_OpenInit(ctx, EVP_aes_256_cbc(), encrypted_key,
encrypted_key_len, iv, priv_key))
handleErrors();

/* Provide the message to be decrypted, and obtain the plaintext output.
 * EVP_OpenUpdate can be called multiple times if necessary
 */
if(1 != EVP_OpenUpdate(ctx, plaintext, &len, ciphertext, ciphertext_len))
handleErrors();
plaintext_len = len;

/* Finalise the decryption. Further plaintext bytes may be written at
 * this stage.
 */
if(1 != EVP_OpenFinal(ctx, plaintext + len, &len)) handleErrors();
plaintext_len += len;

/* Clean up */
EVP_CIPHER_CTX_free(ctx);

return plaintext_len;
}


##Libcrypto接口
OpenSSL提供了两个主要的函数库：libssl和libcrypto。libcrypto库提供了基本的关于密码学的一些常规函数，并且被libssl库所调用。但是你也可以仅使用libcrypto库而不使用libssl库。	

###开始   
要使用libcrypto库，要将它放在开头位置  
    

```c                      
 #include <openssl/conf.h>
 #include <openssl/evp.h>
 #include <openssl/err.h>

 int main(int arc, char *argv[])
{ 
/* Load the human readable error strings for libcrypto */
ERR_load_crypto_strings();

/* Load all digest and cipher algorithms */
OpenSSL_add_all_algorithms();

/* Load config file, and other important initialisation */
OPENSSL_config(NULL);

/* ... Do some crypto stuff here ... */

/* Clean up */

/* Removes all digests and ciphers */
EVP_cleanup();

/* if you omit the next, a small leak may be left when you make use of the BIO (low level API) for e.g. base64 transformations */
CRYPTO_cleanup_all_ex_data();

/* Remove error strings */
ERR_free_strings();

return 0;
}
```
###高级与低级接口
                                                                    
	
对于大多数对Libcrypto库的使用，使用者需要用高级的接口来进行加密解密的操作。它被称为EVP接口（Envelop的简称）。这个接口提供了一系列的功能包括加密解密（对称与非对称),签名与验证，也集合了一些哈希和加密哈希函数的代码，提供算法和模板。使用高级的接口意味着很多关于密码的复杂操作是不可见的。提供了一个单独不变的接口。如果你需要改变你的代码，比如另使用一个算法，那么这是一个很小的改变如果你使用的是高级接口。除此之外，底层的事务比如填充以及加密都有给你准备好模板。

参考EVP页面得到更多的关于高级接口的信息。

除了高级接口，OpenSSL还提供了低级的接口来直接使用个别的算法。这些低级的接口并不推荐给新手使用者，但是提供了可控的范围当只使用高级接口不太合适的时候。注意当你使用FIPS模式时很多低级接口并不可用。

###错误控制
大多数OpenSSL的功能函数会返回一个整数来表明成功或者失败。比如一个函数会返回1表示成功，0表示失败。所有的返回都必须视情况检查和控制。

普遍的是像下面的方式来控制错误。


```c
if(1 != EVP_xxx()) goto err;
if(1 != EVP_yyy()) goto err;

/* ... do some stuff ... */

err:
ERR_print_errors_fp(stderr);
```
注意并不是所有的libcrypto函数会返回0表示失败1表示成功。有些异常会绊倒粗心的程序员。例如：你想要检查一个签名是否正确通过函数返回1表示正确，0表示错误，-1表示一些坏事情发生了，像存储分配错误。所以如果你做：


```c
if (some_verify_function())
/* signature successful */
```
如果有人能够加入一个坏事情发生的情况你最后也把它当作坏的，即使它是好的。这个突然出现在库里面并且被安装在安全释放中。这时你要做的就是查看使用手册或者它的来源。

避免被这种潜在的错误烦恼的一个方法是经常使用这个习惯去检查错误当调用一个OpenSSL函数时：


```c
 if (1 != some_openssl_function())
 /* handle error */

参考Libcrary Errors页面得到更多的关于OpenSSL错误的信息

###线程安全

OpenSSL现在默认是线程不安全的。为了使它线程安全，调用者必须提供各种各样的信号来锁住，原子级的整数加减，以及线程ID测定（最后一个有合理的默认值）。这使得在一个多进程程序里使用OpenSSL变得困难：必须有一个进程提供返回信号。

现在函数库使用OpenSSL保证线程安全唯一的适当的安全的方法是做下面的事情越早越好。

未来我们希望OpenSSL能够自己线程安全来使用简单的线程在适当的地方。



1. 检查信号锁是否设置，没有就设置。
2. `CRYPTO_w_loc(CRYPTO_LOCK_DYNLOCK)`，设置剩下的信号锁 （`threadid`, `dynlock`, 和· `add_lock`），如果未设置，那么`CRYPTO_w_unlock(CRYPTO_LOCK_DYNLOCK)`;

###进程安全
   
主要介绍的文章：Random fork-safety

OpenSSL库的函数主要是异步信号不安全的，因此：

* 不要从信号控制函数中调用OpenSSL函数
* 不用在子进程中调用（exec或者_exit）
* 不要调用OpenSSL从pthread_atfork() handlers 它本身必须是异步信号安全的。

如果你有一个应用要使用OpenSSL在父进程和子进程（没有exec）并且没有正常的放大，那么你在使用其它RAND函数前需要在子进程中调用RAND_poll()

###更多的函数库信息
一些其它的网页覆盖了关于libcrypto具体的方面

* 密码操作的高级接口
* 密码算法
* 输入输出系统
* 椭圆曲线密码学





                                                                  



