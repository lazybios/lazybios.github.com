---
layout: post
title: "加密数据解密算法(Ruby版实现) | 小程序"
date: "2017-01-10"
---

小程序与原生APP之间可以通过微信第三方登录授权进行账号打通，这篇就来说说如何使用Ruby语言对接口返回的加密数据进行对称解密。

小程序官方文档中，给出的解密算法如下:

+ 对称解密使用的算法为 AES-128-CBC，数据采用PKCS#7填充。
+ 对称解密的目标密文为 Base64_Decode(encryptedData),
+ 对称解密秘钥 aeskey = Base64_Decode(session_key), aeskey 是16字节
+ 对称解密算法初始向量 iv 会在数据接口中返回。

描述看起来不太易于理解，不过好在微信官方提供了4类语言的解密demo(C++、Node、PHP、Python)，显然这里面忽略了Ruby，于是在这里补充一份，代码如下:

```ruby
require "openssl"
require "base64"
require "json"


class WXBizDataCrypt
  attr_accessor :app_id, :session_key

  def initialize(app_id, session_key)
    @app_id = app_id
    @session_key = session_key
  end
  
  def decrypt(encrypted_data, iv)
    session_key = Base64.decode64(@session_key)
    encrypted_data= Base64.decode64(encrypted_data)
    iv = Base64.decode64(iv)

    cipher = OpenSSL::Cipher::AES128.new(:CBC)
    cipher.decrypt
    cipher.key = session_key
    cipher.iv = iv

    decrypted = JSON.parse(cipher.update(encrypted_data) + cipher.final)
    raise('Invalid Buffer') if decrypted['watermark']['appid'] != @app_id

    decrypted
  end
end
```

用法跟上面四类语言给出的范例一致，碍于篇幅的限制，demo代码放到了Github上，有兴趣的可以文末搭梯子自行翻阅。

-完-

### 参考引用
+ [https://git.io/vMB8o](https://git.io/vMB8o)
+ [http://t.cn/RMiehuu](http://t.cn/RMiehuu)