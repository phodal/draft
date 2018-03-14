终于，我编写了自己的云同步的随机密码器。。。
===

作为一个专业的咨询师，一年多以前，我重新考虑了我的密码策略。在这一年多里，我在一些场合里，使用了自己的随机密码器，它还是“云同步”的。

取一个变量很纠结，取一个密码很头痛，为此我们并不能取一个简单的密码。

![硅谷](silicon-valley.jpg)

于是，我终于编写了自己的云同步的随机密码器。。。

不过是出于以下的背景：

1. 10 年前我使用一个固定的、统一的密码，直到一系列的明文密码泄露事件，我在不同的平台采用了不同的密码。
2. 在经历了一系列的忘记密码之后，我开始采用平台限定的密码，即不同的平台，密码是半动态的。
3. 现今的大部分重要网站都采用了二次验证，或是 Google Authenticator，或是短信验证。
4. 在 ThoughtWorks，我们的密码需要每三个月换一次，它足够的长，并且有一系列的复杂规则。


快速密码
---

```
1234qwer
```

```
1qaz2wsx
```

按键

```
1qaz@WSX
```

特定平台
---


京东：``tb-1qaz2wsx``

淘宝：``jd-1qaz2wsx``



使用：SHA-1 生成
---


当然了 ``btoa('password')``，也是一个不错的建议：``cGFzc3dvcmQ=``。

```
commit b40c83cb772d4b496d1f9ecec136e21d46f6a765
Author: Phodal HUANG <h@phodal.com>
Date:   Tue Mar 13 17:00:11 2018 +0800

    [mooa] log: add basic logic

commit 3c8badf0bd6671368910fb22eada5a850a3fcf88
Author: Phodal HUANG <h@phodal.com>
Date:   Tue Mar 13 16:46:53 2018 +0800

    [mooa] rename staus filter to status helper
```

生成密码
---

```
from random import choice
from string import hexdigits, punctuation
import time

base_time = str(time.time())[12:16]
base_string = ''.join(choice(hexdigits) for i in range(6))
base_punctuation = ''.join(choice(punctuation) for j in range(2))

print(base_string + base_punctuation + base_time)
```

```
1520944501.967865
```

```
0123456789abcdefABCDEF
```

```
!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
```

```
09a86a{#4242
```

我使用的是 Python 3，你们这些 Python 2.6 的异教徒。


二次验证的云同步
---

LassPass、1Password

1Password 可以使用 iCloud 同步，可惜我是个 Android 用户。并且 1Password 也有对应的 Chrome 插件，可是 Chrome 的自动填充是另外一个问题。

所以，尽量不要把电脑借给别人。

1Password 的密码策略，让我觉得它也是不可靠的——Master Password，也就是一个密码可以解锁其它密码。

于是，最后我采用的策略是，半动态的密码 + 生成的密码。



![Password in Latop](password-in-latop.jpg)
