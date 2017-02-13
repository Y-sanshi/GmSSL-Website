# 国密SM4/SMS4分组密码

SMS4是我国无线局域网标准WAPI中所采用的分组密码标准，随后被我国商用密码标准采用，又名SM4（SM是“商密”的缩写，目前公布的其他商密标准包括SM2椭圆曲线公钥密码、SM3密码杂凑算法）。作为我国商用密码的分组密码标准，预计SMS4在国内的敏感但非机密的应用领域会逐渐取代3DES、AES等国外分组密码标准，用于通信加密、数据加密等应用场合。

SMS4的密钥长度和分组长度均为128比特，其设计安全性等同于AES-128，但是近年来的一些密码分析表明SMS4的安全性略弱于AES-128。由于SMS4的密钥长度固定为128比特，并有提供更长的可选密钥长度，因此SMS4不适用于保护需长期保密的数据，如需50年才能解密的保密文档。

SMS4算法存在一定的性能问题。由于SMS4设计时的预计应用领域为低功耗芯片(即WAPI芯片)，因此SMS4针对减少硬件电路数量进行了优化，带来的后果是SMS4的软件实现效率较低，难以充分利用主流32位/64位通用处理器的计算能力，其软件实现的效率通常大大低于AES-128的软件实现。

GmSSL提供了SMS4的实现。

## SMS4的工作模式

GmSSL已经实现如下工作模式：

* SMS4-ECB，该模式不推荐
* SMS4-CBC，该模式的实现提供自动的填充，无需应用对明文数据进行填充。
* SMS4-CFB，根据输出比特序列的长度，包含SMS4-CFB1、SMS4-CFB8和SMS4-CFB128三个实现。
* SMS4-OFB
* SMS4-CTR，由于SMS4软实现性能较低，因此在后续的优化中会首先提供经过Intel AVX2指令集优化的CTR实现。
* SMS4-WRAP，将SMS4用于加密密钥，其中被加密的数据为密钥，而SMS4的密钥为KEK (Key Encryption Key)。

GmSSL预期还将实现如下工作模式：

* SMS4-GCM，该模式同时提供数据加密以及完整性保护，通过该模式在加密数据时无需再提供格外的HMAC。
* SMS4-OCB，该模式功能与GCM模式相同。
* SMS4-XTS，该模式为磁盘加密模式，用于加密以512字节、4096字节为单位的磁盘扇区。
* SMS4-FFX，该模式为保留格式加密，可以用于加密信用卡号码等具有特定格式的字符串。


## 在命令行中使用SMS4

调用SMS4的命令行例子如下：

```
$ echo hello | gmssl enc -sms4-cbc > ciphertext.bin
enter sms4-cbc encryption password:********
Verifying - enter sms4-cbc encryption password:********

$ cat cipehrtext.bin | gmssl enc -sms4-cbc -d
enter sms4-cbc decryption password:********
hello
```

GmSSL提供了SMS4的`EVP_sms4_cbc()`等`EVP_CIPHER`对象，应用可以通过`EVP_EncryptInit/Update/Final()`函数访问，具体调用方法请参考`EVP_EncryptInit(3)`手册页。


