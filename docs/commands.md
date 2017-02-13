显示GmSSL版本

```
gmssl version
```

生成SM2密钥

```
gmssl genkey -algorithm EC -out sm2key.pem \
        -pkeyopt ec_paramgen_curve:sm2p256v1 \
        -pkeyopt ec_param_enc:named_curve
```

打印SM2密钥

```
gmssl pkey -text -noout -in sm2key.pem
```

导出SM2公钥

```
gmssl pkey -in sm2key.pem -pubout -out sm2pubk.pem
```

生成SM2数字签名

```
echo "message to be signed" | \
gmssl pkeyutl -sign -inkey sm2key.pem \
        -pkeyopt ec_sign_algor:sm2 > sm2.sig
```

验证SM2数字签名

```
echo "message to be signed" | \
gmssl pkeyutl -verify -inkey sm2pubk.pem \
        -pkeyopt ec_sign_algor:sm2 -sigfile sm2.sig
```

