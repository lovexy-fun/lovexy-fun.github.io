---
title: via浏览器和Alook浏览器插件安装
date: 2021-04-16 17:41
updated: 2021-04-16 17:41
tags:
  - 瞎搞
---

via和Alook是Android和IOS上可以支持JS插件的浏览器，一些常用的插件可以在via-app.cn上找到。但总会有人会思考点击安装按钮的是怎样将JS脚本代码安装到浏览器的。

经过对页面代码的分析得到一下结论：

浏览器向`window`上添加了一个`via`对象，安装某个脚本只需要调用方法即可，具体调用方法为：

```javascript
window.via.addon(Base64字符串)
```

**Base64字符串**是由固定格式的json字符串转码而来的，json格式如下：

```json
{
    "author": "作者，字符串格式，ASCII编码，例如：\u8fd9\u662f\u4e2a\u4f8b\u5b50",
    "code": "JS脚本Base64编码后的字符串",
    "id": 1,
    "name": "插件名称，字符串格式，同样是ASCII编码",
    "url": "匹配的网址，一般使用*"
}
```

说明：

1. 对于via浏览器来说author和name是非必须项，id、code和url为必须项。
2. 对于Alook浏览器来说author、name和id是非必须，code和url都存在时插件识别为被动插件，只有code时识别为主动插件。
3. 目前没有准确的文档，或许会存在一些上述格式中不存在的字段。

其他补充：
1. alook浏览器必须使用带填充base64字符串，例如：`alert()`不带填充编码为`YWxlcnQoKQ`带填充编码为`YWxlcnQoKQ==`。
