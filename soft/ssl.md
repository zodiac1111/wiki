基于OpenSSL自建CA和颁发SSL证书
http://my.oschina.net/aiguozhe/blog/121764


基于OpenSSL自建CA和颁发SSL证书


http://drops.wooyun.org/tips/6403
openresty+lua在反向代理服务中的玩法| WooYun知识库

`openssl genrsa -des3 -out ssl.key 1024`

```
mv ssl.key xxx.key
openssl rsa -in xxx.key -out ssl.key
rm xxx.key
```

`openssl req -new-key ssl.key -out ssl.csr`


`openssl x509 -req -days 365 -in ssl.csr -signkey ssl.key -out ssl.crt`