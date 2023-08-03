# project-17
比较Google和Firefox记住密码插件实现方式及其区别

谷歌（Google）的Chrome和Mozilla的Firefox作为两个主要的网络浏览器，也提供了记住密码的功能，但它们在实现方式上可能存在一些区别。本次实验报告旨在探讨Google和Firefox浏览器中的记住密码插件实现方式，并比较它们之间的区别，重点分析网络安全原理和密码学知识在其中的应用。

# Google Chrome的记住密码插件实现
Google Chrome在实现记住密码插件时，采用了加密的本地数据库来存储用户密码。为了保证密码的安全性，Chrome依赖于操作系统级别的加密服务。具体来说，Chrome使用Windows操作系统的CryptProtectData API或macOS和Linux操作系统的相应加密服务，将用户密码进行加密存储。这些加密服务利用操作系统中的密钥链来保护用户密码，确保只有经过身份验证的用户才能访问密码。

Chrome的记住密码插件采用了数据保护的原则。它使用操作系统提供的加密服务，将敏感数据（密码）加密存储在本地设备上，防止未经授权的访问者轻易获得原始密码。此外，Chrome支持双重身份验证，进一步提高了账户的安全性。

例如用户登录谷歌Chrome后，启用了双重身份验证。在下次访问保存的密码时，Chrome将要求用户提供一次性验证码，以确保只有正确的用户能够访问保存的密码。

此外Chrome使用了对称加密算法，如AES（Advanced Encryption Standard）或Triple DES（Triple Data Encryption Standard）。这些算法使用相同的密钥进行加密和解密，而密钥本身是由操作系统提供的密钥链服务保护的。这种密码学知识保证了加密和解密的安全性。

#  Firefox的记住密码插件实现

Mozilla Firefox在实现记住密码插件时，采用了自有的加密数据库来存储用户密码。为了保证密码的安全性，Firefox使用了PKCS#11技术，该技术用于存储证书和密钥。用户可以选择启用主密码功能，这样在访问保存的密码之前，需要提供主密码进行解密。

Firefox的记住密码插件同样采用了数据保护的原则。它使用自有的加密数据库和PKCS#11技术，将用户密码加密存储在本地设备上。启用主密码功能后，用户需要提供主密码进行解密，从而保护了保存的密码免受未经授权的访问。


在加密用户密码时，Firefox使用了与操作系统无关的加密算法。它使用PKCS#11技术存储证书和密钥，并采用对称加密算法来保护用户密码。主密码功能则增加了一层额外的保护，这种密码学知识确保了密码的安全性。



# 实现方式的区别
技术架构上谷歌的Chrome更依赖于操作系统的安全架构，使用操作系统提供的密钥链服务进行加密；而Mozilla的Firefox采用了更独立的解决方案，使用自有的加密数据库和PKCS#11技术。
谷歌的Chrome更依赖于操作系统的加密服务，而Mozilla的Firefox采用了自有的加密数据库和PKCS#11技术，更独立于操作系统。

在声明权限时，Firefox使用"permissions"字段，而谷歌浏览器使用"permissions"字段或"optional_permissions"字段来声明不同类型的权限。

firefox:
```js
  "manifest_version": 2,
  "name": "Your Password Manager",
  "version": "1.0",
  "description": "A password manager extension for Firefox",
  "permissions": [
    "tabs",
    "storage"
  ],
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content.js"]
    }
  ],
  "browser_action": {
    "default_icon": "icon.png",
    "default_popup": "popup.html"
  }
```

google:
```js
"manifest_version": 2,
  "name": "Your Password Manager",
  "version": "1.0",
  "description": "A password manager extension for Chrome",
  "permissions": [
    "tabs",
    "storage"
  ],
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content.js"]
    }
  ],
  "browser_action": {
    "default_icon": "icon.png",
    "default_popup": "popup.html"
  }
```

在获取当前选项卡信息时，Firefox使用'browser.tabs' API，而谷歌浏览器使用'chrome.tabs' API。
firefox:
```js
browser.tabs.query({ currentWindow: true, active: true }, function(tabs) {
  const currentTab = tabs[0];
});
```

google:
```js
chrome.tabs.query({ currentWindow: true, active: true }, function(tabs) {
  const currentTab = tabs[0];
});
```
对登录表单提交事件的监听和内容脚本的注入的不同
firefox:
```js
document.addEventListener("submit", function(event) {
});
```

google:
```js
document.addEventListener("submit", function(event) {
});
```
# 结论
Google和Firefox的记住密码插件在功能上虽相似，但在技术实现和用户体验方面存在显著差异。谷歌的Chrome更依赖于操作系统的安全架构，而Mozilla的Firefox采用了更独立的解决方案。双重身份验证和主密码功能都是重要的安全措施，提高了密码的安全性。在使用这些记住密码插件时，用户应该合理配置密码管理设置，并保持操作系统和浏览器的更新，以确保账户的安全。总体而言，Google和Firefox的记住密码插件都为用户提供了便利，但用户应该在便利和安全之间做出权衡。
# 实验环境
| 语言  | 系统      | 平台   | 处理器                     |
|-------|-----------|--------|----------------------------|
| Cpp   | Windows10 | pycharm| Intel(R) Core(TM)i7-11800H |
# 小组分工
戴方奇 202100460092 单人组完成project17


