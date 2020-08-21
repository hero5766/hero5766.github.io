title: ts文件解密
author: hero576
tags:
  - python
categories:
  - programme
date: 2020-08-07 21:07:00
---
> 通过m3u8下载视频文件，下载完成后无法播放。查看m3u8文件，发现视频文件被加密，所以直接下载后不能直接播放。
<!--more-->

# ts文件加密方式

|AES加密方式||
|-|-|
|ECB|Electronic CodeBook，电子密码本模式|
|CBC|Cipher-block chaining，密码块连接模式|
|PCBC|Propagating cipher-block chaining，填充密码块链接模式|
|CTR|Counter mode，计数器模式|
|CFB|Cipher feedback，密文反馈模式|
|OFB|Output feedback，输出反馈模式|

- [加密方式详细介绍](https://blog.csdn.net/u013073067/article/details/87086562)

# python实现加密算法
- 安装环境
```bash
#Windows
pip install pycryptodome 
#Linux
pip install pycrypto 
```

## CBC
- cbc模式使用了AES-128加密，需要一个十六位的key(密钥)和一个十六位iv(偏移量)
```python
from Crypto.Cipher import AES
import os
from Crypto import Random
import base64
class AESUtil:
    __BLOCK_SIZE_16 = BLOCK_SIZE_16 = AES.block_size
    @staticmethod
    def encryt(data, key, iv=None):
        bs = AES.block_size
        pad = lambda s: s + ((bs - len(s) % bs) * chr(bs - len(s) % bs)).encode()
        if not iv:
            iv = Random.new().read(bs)
        cipher = AES.new(key, AES.MODE_CBC, iv)
        data = cipher.encrypt(pad(data))
        data = iv + data
        return data
    @staticmethod
    def decrypt(data, key, iv=None):
        bs = AES.block_size
        if len(data) <= bs:
            return data
        #unpad = lambda s : s[0:-ord(s[-1])]
        unpad = lambda s : s[0:-(s[-1])]
        if not iv:
            iv = data[:bs]
        cipher = AES.new(key, AES.MODE_CBC, iv)
        data  = unpad(cipher.decrypt(data[bs:]))
        return data
if __name__ == "__main__":
    key = "1234567812345678"
    iv = "1234567812345678"
    res = AESUtil.encryt("123456", key, iv)
    print(res) # 2eDiseYiSX62qk/WS/ZDmg==
    print(AESUtil.decrypt(res, key, iv)) # 123456
```







