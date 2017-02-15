# 开发路线图

## GmSSL-2.x

GmSSL-2.x版本的预计开发和维护时间约为1年，主要实现如下目标：

1. 代码迁移至OpenSSL-1.1.0，支持编译选项和单元测试，支持macOS/Linux/Windows。
2. 支持密码硬件和加速指令集，包括Intel AVX-512/KNC-NI/AVX-2指令集、国密PCI-E加密卡、国密USB-KEY和VIA-Alliance芯片国密指令集。
3. 支持SM9、BF-IBE和BB1-IBE。
4. 支持国密证书和CA，以及基于国密证书的SSL通信。

