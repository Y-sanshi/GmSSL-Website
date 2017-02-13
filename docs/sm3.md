SM3是国密密码杂凑算法标准，由国家密码管理局于2010年12月公布。SM3的输出杂凑值长度为256比特(32字节)，与国际标准SHA-256等长。SM3设计安全性为128比特，安全性与256比特椭圆曲线/SM2、SM4/SMS4、AES-128等同。

可以通过GmSSL命令行工具计算SM3杂凑值和HMAC消息认证码。

下面的例子验证了SM3标准文本中给出的两个测试用例：

```
$ echo -n abc | gmssl dgst -sm3
(stdin)= 66c7f0f462eeedd9d1f2d46bdc10e4e24167c4875cf2f7a2297da02b8f4ba8e0
$ echo -n abcdabcdabcdabcdabcdabcdabcdabcdabcdabcdabcdabcdabcdabcdabcdabcd | \
          gmssl dgst -sm3
(stdin)= debe9ff92275b8a138604889c18e5a4d6fdb70e5387e5765293dcba39c0c5732
```

计算HMAC-SM3：
```
$ echo -n abc | gmssl dgst -sm3 -hmac Passw0rd
(stdin)= db1ab0dda0aafbdcd53cbda95b7ecdee4a50586f92696616ab052aceea106212
```

GmSSL提供了`EVP_sm3()` EVP_MD对象，应用程序可以通过标准的EVP API计算SM3杂凑值，具体接口调用方式可参考`man EVP_DigestInit`。

