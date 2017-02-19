# EVP API

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
	/ 设置一个2048位的素数 */
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




