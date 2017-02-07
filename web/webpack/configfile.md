entry
-
表示程序的启动文件，也叫做入口文件，可以是三种情况

1.直接跟字符串
```js
    entry: 'index.js'
```
这就表示入口文件只有一个，模块打包出来的name还是index

2.跟一个数组
```js
    entry: ['index1.js', 'index2.js']
```
这种情况下，所有的模块都会在启动的时候加载，但是打包出来的name是output.filename指定的名称

3.跟一个对象
```js
    entry: {
        file1: 'index1.js',
        'file2': ['index2.js'.'index3.js']
    }
```
这种情况下，如果对象的key是字符串，name打包出来的name是字符串的内容，如果字符串内有`/`符号，会编译成文件夹路径。 如果对象的key不是字符串或者字符串中没有`/`符号，他会编译成打包后的name

注意事项：
-
如果在resolve.extensions中定义了可以省略的扩展名，入口文件中的文件，不能加扩展，否则会报错
