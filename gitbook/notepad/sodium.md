> centos 安装 sodium
```
yum -y groupinstall "Development Tools"
yum install unzip autoconf automake libtool -y
wget --no-check-certificate -O "libsodium-master.zip" https://github.com/jedisct1/libsodium/archive/master.zip
unzip libsodium-master.zip && cd libsodium-master
./autogen.sh && ./configure --disable-maintainer-mode && make -j2 && make install
echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf
ldconfig
cd .. && rm -rf libsodium-master.zip && rm -rf libsodium-master
```

> php 安装sodium扩张
```
pecl install libsodium
```