执行

```
root@ubuntu:~# sudo apt-get install ngrok-server
```

然后

```
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following packages were automatically installed and are no longer required:
  linux-headers-4.4.0-62 linux-headers-4.4.0-62-generic linux-headers-4.4.0-83 linux-headers-4.4.0-83-generic linux-headers-4.4.0-87
  linux-headers-4.4.0-87-generic linux-image-4.4.0-62-generic linux-image-4.4.0-83-generic linux-image-4.4.0-87-generic linux-image-extra-4.4.0-62-generic
  linux-image-extra-4.4.0-83-generic linux-image-extra-4.4.0-87-generic
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  binutils build-essential cpp cpp-5 dpkg-dev fakeroot g++ g++-5 gcc gcc-5 libalgorithm-diff-perl libalgorithm-diff-xs-perl libalgorithm-merge-perl
  libasan2 libatomic1 libc-dev-bin libc6-dev libcc1-0 libcilkrts5 libdpkg-perl libfakeroot libfile-fcntllock-perl libgcc-5-dev libgomp1 libisl15 libitm1
  liblsan0 libmpc3 libmpx0 libquadmath0 libstdc++-5-dev libtsan0 libubsan0 linux-libc-dev make manpages-dev
Suggested packages:
  binutils-doc cpp-doc gcc-5-locales debian-keyring g++-multilib g++-5-multilib gcc-5-doc libstdc++6-5-dbg gcc-multilib autoconf automake libtool flex
  bison gdb gcc-doc gcc-5-multilib libgcc1-dbg libgomp1-dbg libitm1-dbg libatomic1-dbg libasan2-dbg liblsan0-dbg libtsan0-dbg libubsan0-dbg libcilkrts5-dbg
  libmpx0-dbg libquadmath0-dbg glibc-doc libstdc++-5-doc make-doc
The following NEW packages will be installed:
  binutils build-essential cpp cpp-5 dpkg-dev fakeroot g++ g++-5 gcc gcc-5 libalgorithm-diff-perl libalgorithm-diff-xs-perl libalgorithm-merge-perl
  libasan2 libatomic1 libc-dev-bin libc6-dev libcc1-0 libcilkrts5 libdpkg-perl libfakeroot libfile-fcntllock-perl libgcc-5-dev libgomp1 libisl15 libitm1
  liblsan0 libmpc3 libmpx0 libquadmath0 libstdc++-5-dev libtsan0 libubsan0 linux-libc-dev make manpages-dev ngrok-server
0 upgraded, 37 newly installed, 0 to remove and 71 not upgraded.
Need to get 39.5 MB of archives.
After this operation, 150 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://us.archive.ubuntu.com/ubuntu xenial/main amd64 libmpc3 amd64 1.0.3-1 [39.7 kB]
Get:2 http://us.archive.ubuntu.com/ubuntu xenial-updates/main amd64 binutils amd64 2.26.1-1ubuntu1~16.04.4 [2,311 kB]
Get:3 http://us.archive.ubuntu.com/ubuntu xenial-updates/main amd64 libc-dev-bin amd64 2.23-0ubuntu9 [68.6 kB]
```

发现是旧版本的：

```
NGROK_DOMAIN="hook.fengda.me"
openssl genrsa -out base.key 2048
openssl req -new -x509 -nodes -key base.key -days 10000 -subj "/CN=$NGROK_DOMAIN" -out base.pem
openssl genrsa -out server.key 2048
openssl req -new -key server.key -subj "/CN=$NGROK_DOMAIN" -out server.csr
openssl x509 -req -in server.csr -CA base.pem -CAkey base.key -CAcreateserial -days 10000 -out server.crt
```

只好到官网下载:

```
root@ubuntu:~/ngrok# unzip ngrok-stable-linux-amd64.zip
The program 'unzip' is currently not installed. You can install it by typing:
apt install unzip
```

安装，执行

```
root@ubuntu:~/ngrok# apt install unzip
root@ubuntu:~/ngrok# unzip ngrok-stable-linux-amd64.zip
```

注册：

```
root@ubuntu:~/ngrok# ./ngrok authtoken 7yn98XuLteq3GCpxNnbzt_6C3FPKPSB4cHEBYR6wT65
Authtoken saved to configuration file: /root/.ngrok2/ngrok.yml
root@ubuntu:~/ngrok#
root@ubuntu:~/ngrok# cat /root/.ngrok2/ngrok.yml
authtoken: 7yn98XuLteq3GCpxNnbzt_6C3FPKPSB4cHEBYR6wT65
```

执行

```
root@ubuntu:~/ngrok# ./ngrok http 80
ngrok by @inconshreveable                                                                                                                    (Ctrl+C to quit)

Session Status                online
Account                       Phodal Huang (Plan: Free)
Version                       2.2.8
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://51247681.ngrok.io -> localhost:80
Forwarding                    https://51247681.ngrok.io -> localhost:80

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       1       0.00    0.00    0.00    0.00

HTTP Requests
-------------

GET /favicon.ico               404 Not Found
GET /                          200 OK
```
