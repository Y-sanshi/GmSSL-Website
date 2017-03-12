# 编译与安装

## GmSSL-2.x

```sh
./config
make
make test
sudo make install
```

如果某个模块无法通过编译，可以通过条件编译选项不编译该模块，如

```sh
./config no-sm9
```


## GmSSL-1.x

### 在Mac下编译和安装

```
./Configure darwin64-x86_64-cc --prefix=/usr/local --openssldir=/usr/local/openssl
make
sudo make install
```

### 在Linux下编译和安装

```
./config --prefix=/usr/local --openssldir=/usr/local/openssl
make
sudo make install
```

### 在Windows下编译和安装

在Developer Command Prompt for VS2013命令行环境中输入：
```
perl Configure VC-WIN32 no-asm --prefix=c:\openssl
ms\do_ms
nmake -f ms\nt.mak
```
