问题：
```
 ruby SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B:
 ```
解决办法。

首先在这里下载证书(http://curl.haxx.se/ca/cacert.pem), 然后再环境变量里设置`SSL_CERT_FILE`这个环境变量，变量值为这个文件的路径。
