GmSSL支持国密《GM/T 0024-2014: SSL VPN技术规范》的新增密码套件：

```
 1 ECDHE-SM1-SM3   0xE001
 2 ECC-SM1-SM3     0xE003
 3 IBSDH-SM1-SM3   0xE005
 4 IBC-SM1-SM3     0xE007
 5 RSA-SM1-SM3     0xE009
 6 RSA-SM1-SHA1    0xE00A
 7 ECDHE-SM4-SM3   0xE011
 8 ECC-SM4-SM3     0xE013
 9 IBSDH-SM4-SM3   0xE015
10 IBC-SM4-SM3     0xE017
11 RSA-SM4-SM3     0xE019
12 RSA-SM4-SHA1    0xE01A
```

GmSSL还包含两个专有的密码套件，以兼容某些仅使用SM2证书但不修改SSL/TLS协议的SSL VPN产品。
```
13 ECDHE-SM2-SM4-SM3
14 SM2-SM4-SM3
```

说明：
 * 通过前12个密钥套件通信时，GmSSL采用《GM/T 0024-2014: SSL VPN技术规范》的协议，即双证书模式。
 * 套件1-6依赖未公开的SM1算法实现，因此需要通过ENGINE访问支持SM1的密码硬件。
 * 采用`ECDHE-SM1-SM3`和`ECDHE-SM4-SM3`密码套件通信时，GmSSL采用带双方证书的SM2密钥交换协议而非标准的ECDH。
 * 采用`ECDHE-SM2-SM4-SM3`通信时，服务器端证书为SM2签名的SM2签名密钥证书，采用SSL/TLS的临时ECDH密钥交换方式。
 * 采用`SM2-SM4-SM3`通信时，服务器端证书为SM2签名的SM2加密密钥证书，采用客户端通过SM2加密预主密钥的密钥传输方式。
 * 采用`ECDHE-SM2-SM4-SM3`和`SM2-SM4-SM3`两个密码套件通信时，GmSSL可以选择配置为采用TLS协议。
 * 上述部分套件存在众所周知的安全特性（安全隐患），需根据应用的需求进行配置选择。
