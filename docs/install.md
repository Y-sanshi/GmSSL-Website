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