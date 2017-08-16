
 - 需要工具链 xtensa-lx106-elf，自己编译 ``make toolchain esptool libhal STANDALONE=n``
 - ESP8266_RTOS_SDK

搭建
---

```
$ brew tap homebrew/dupes
$ brew install binutils coreutils automake wget gawk libtool help2man gperf gnu-sed --with-default-names grep
$ export PATH="/usr/local/opt/gnu-sed/libexec/gnubin:$PATH"
```

In addition to the development tools MacOS needs a case-sensitive filesystem. You might need to create a virtual disk and build esp-open-sdk on it:

```
$ sudo hdiutil create ~/Documents/case-sensitive.dmg -volname "case-sensitive" -size 10g -fs "Case-sensitive HFS+"
$ sudo hdiutil mount ~/Documents/case-sensitive.dmg
$ cd /Volumes/case-sensitive
```

编译
---

```
➜  test git clone --recursive https://github.com/pfalcon/esp-open-sdk.git
```

```
Cloning into 'esp-open-sdk'...
remote: Counting objects: 530, done.
remote: Total 530 (delta 0), reused 0 (delta 0), pack-reused 530
Receiving objects: 100% (530/530), 332.20 KiB | 73.00 KiB/s, done.
Resolving deltas: 100% (305/305), done.
Submodule 'crosstool-NG' (https://github.com/jcmvbkbc/crosstool-NG) registered for path 'crosstool-NG'
Submodule 'esp-open-lwip' (https://github.com/pfalcon/esp-open-lwip) registered for path 'esp-open-lwip'
Submodule 'esptool' (https://github.com/themadinventor/esptool) registered for path 'esptool'
Submodule 'lx106-hal' (https://github.com/tommie/lx106-hal) registered for path 'lx106-hal'
Cloning into '/Users/phodal/test/esp-open-sdk/crosstool-NG'...
remote: Counting objects: 33801, done.
remote: Total 33801 (delta 0), reused 0 (delta 0), pack-reused 33800
Receiving objects: 100% (33801/33801), 17.08 MiB | 163.00 KiB/s, done.
Resolving deltas: 100% (19981/19981), done.
Cloning into '/Users/phodal/test/esp-open-sdk/esp-open-lwip'...
remote: Counting objects: 236, done.
remote: Total 236 (delta 0), reused 0 (delta 0), pack-reused 236
Receiving objects: 100% (236/236), 495.14 KiB | 70.00 KiB/s, done.
Resolving deltas: 100% (75/75), done.
Cloning into '/Users/phodal/test/esp-open-sdk/esptool'...
remote: Counting objects: 1323, done.
remote: Total 1323 (delta 0), reused 0 (delta 0), pack-reused 1323
Receiving objects: 100% (1323/1323), 4.86 MiB | 137.00 KiB/s, done.
Resolving deltas: 100% (796/796), done.
Cloning into '/Users/phodal/test/esp-open-sdk/lx106-hal'...
remote: Counting objects: 122, done.
remote: Total 122 (delta 0), reused 0 (delta 0), pack-reused 122
Receiving objects: 100% (122/122), 179.61 KiB | 46.00 KiB/s, done.
Resolving deltas: 100% (51/51), done.
Submodule path 'crosstool-NG': checked out '37b07f6fbea2e5d23434f7e91614528f839db056'
Submodule path 'esp-open-lwip': checked out '8c39d2179a273553466043f388772abb6251a4ca'
Submodule path 'esptool': checked out '9dfcb350e1a91bb4641f725fc6c2f126791013ce'
Submodule path 'lx106-hal': checked out 'e4bcc63c9c016e4f8848e7e8f512438ca857531d'
```
