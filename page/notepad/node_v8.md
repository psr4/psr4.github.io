```
cd /usr/local/src/

wget https://npm.taobao.org/mirrors/node/v8.0.0/node-v8.0.0-linux-x64.tar.xz

tar -xvf  node-v8.0.0-linux-x64.tar.xz

cd  node-v8.0.0-linux-x64/bin && ls

ln -s /usr/local/src/node-v8.0.0-linux-x64/bin/node /usr/local/bin/node

ln -s /usr/local/src/node-v8.0.0-linux-x64/bin/npm /usr/local/bin/npm

node -v

npm -v

npm install -g pm2

ln -s /usr/local/src/node-v8.0.0-linux-x64/bin/pm2  /usr/local/bin/pm2
```