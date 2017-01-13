multer介绍
-
multer是一个NodeJS中间件，用来处理multipart/form-data类型的表单数据，主要用于上传文件的时候，看了一些文档，感觉介绍的都不够清晰

安装和使用
-
```js
    // 安装 
    npm i multer --save
    // 使用
    var multer = require('multer');
```
那么multer中间件的原理是什么？他会给req添加一个files对象，这个对象包含通过表单上传的文件信息,这个files对象有三种各类型，我们先大致看一下，通过中间件获取的文件信息都有哪些：

key | description| 信息存储在哪里
--- | ---|---
fieldname|表单中文件空间的name属性|
originnalname|上传的文件名称|
encoding|文件编码|
mimetype|文件的Mime类型|
size|文件大小，以字节为单位|
destination|保存路径的目录|硬盘
filename|保存的临时文件名|硬盘
path|保存路径的完整路径(目录+文件名)|硬盘
buffer|整个文件的Buffer数据|内存

三种获取文件信息的方式：
```js
    app.post('upload_cache/', multer().single(),function(req,res){
        console.log(req.file) //获取上传的文件信息
        })
```
上面的方法需要使用req.file来获取上传的文件，只适用于上传单个文件,single()需要填写正确的文件field_name，否则会报错
```js
    app.post('upload_cache/', multer().array(),function(req,res){
        console.log(req.files) //获取上传的文件信息
        })
```