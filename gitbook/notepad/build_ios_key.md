-准备OpenSSL
```
访问：http://slproweb.com/products/Win32OpenSSL.html 。并下载Win32 OpenSSL v1.0.1c Light版本（注意：版本可能会升级），
如果您运行OpenSSL有问题，还需要下载Visual C++ 2008 Redistributables安装。
安装OpenSSL完毕后，默认在C盘的OpenSSL-Win32。
```
-生成CSR请求文件
```
进入Windows的命令行（WIN+R，进入运行）。开始输入各个命令.
cd C:\OpenSSL-Win32\bin\
set RANDFILE=.rnd
set OPENSSL_CONF=C:\OpenSSL-Win32\bin\openssl.cfg
openssl genrsa -out my.key 2048
openssl req -new -key my.key -out my.certSigningRequest -subj "/emailAddress=myemail@sample.com,CN=Common Name,C=CN"
```
-获取CER证书
```
将生成的certSigningRequest文件上传到Apple的开发者相关页面（例如开发者证书、推送证书等）
https://developer.apple.com/ios/manage/certificates/team/distribute.action
将最后生成的cer文件改个名字后放到C:\OpenSSL-Win32\bin\目录中，和之前生成的文件放在一起。
```
-导出p12文件
```
P12文件包含了证书的密钥和公钥，可以方便迁移到其他电脑上。 最后在刚才的环境中运行命令行（如果之前命令行窗口被关了，还是要重新执行一遍开始的几条set环境配置命令）：
openssl x509 -in my.cer -inform DER -out my.pem -outform PEM
openssl pkcs12 -export -inkey my.key -in my.pem -out my.p12 -password pass:111111
这样就生成了密码为111111，文件名为my.p12的密钥文件。您可以将这个文件导入MAC电脑，也可以上传到追信魔盒的IOS代签名区。
```

