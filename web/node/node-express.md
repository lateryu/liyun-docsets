安装使用
-
```js
    npm i express --save
```
```js
    var express = require('express');
    var app = express();
```

req.query
-
当请求模式为GET的时候，这个方法用来获取传递过来的参数，其实也就是url上的search部分，如果没有参数传递过来，他会返回一个空的对象{},

url上的参数都是字符串类型的，这个方法能返回一个对象类型没说明他直接字符串参数都转成JSON对象了

如果是ajax传递过来的参数，这个方法既可以解析到url上的参数，又可以解析到ajax.data上传递过来的参数

当请求模式为POST的时候，这个方法也返回空对象

req.body
-
当请求模式为POST的时候，这个方法用来获取传递过来的参数，包括ajax过来的参数, 如果没有参数，会返回一个空的对象。

当模式为GET的时候，这个方法返回空对象,
