babel的主要目的就一句话，将ES6代码转为ES5代码，从而在现有环境内可以执行。

安装babel
-
可以使用npm命令直接安装babel包

命令行中使用babel
-
这种方式只需要在全局环境下安装 babel-cli包即可

    npm install babel-cli -g
安装后在命令行中可直接通过babel命令编译ES6代码，例如

转码结果输出到标准输出

    $ babel example.js

转码结果写入一个文件, --out-file 或 -o 参数指定输出文件

    $ babel example.js --out-file compiled.js
    或者
    $ babel example.js -o compiled.js

整个目录转码

    --out-dir 或 -d 参数指定输出目录
    $ babel src --out-dir lib
    或者
    $ babel src -d lib

-s 参数生成source map文件

    $ babel src -d lib -s
在项目中使用babel
-
命令行使用babel，缺点是对全局环境产生了依赖 ，不建议使用这个方式。我们可以把babel-cli安装在项目环境中，通过配置package.json文件来使用babel就可以解决这个问题

    npm install --save-dev babel-cli
`package.json`:

    {
        "scripts": {
            "build": "babel example.js"
        }
    }
在项目中使用babel不要忘记安装babel转码基础包：
    
    ES5转码规则：
    npm install --save-dev babel-preset-es2015

    ES7不同阶段的提案转码规则，可任选一个安装：
    npm install --save-dev babel-preset-stage-0
    npm install --save-dev babel-preset-stage-1
    npm install --save-dev babel-preset-stage-2
    npm install --save-dev babel-preset-stage-3

    有的时候我们需要react的转码规则，也可以自行安装
    npm install --save-dev babel-preset-react
babel配置文件
-
当然了，在项目中使用babel不要忘记添加babel的配置文件.babelrc

    {
        "presets": [
          "es2015",
          "react",
          "stage-2"
        ],
        "plugins": [],
        "ignore":[]
    }
其中presets里面是我们要用到的转码规则，plugins的作用暂未可知，ignore指的是在转码的时候要忽略的文件
babel-node
-
babel-node是babel-cli中自带的一个命令行工具，里面可以直接运行JS代码，不用单独安装

babel-register(暂未了解使用的结果)
-
这个包会改写require命令，引入这个包之后，每当使用require加载.js,.jsx,.es,.es6等后缀名的文件时，就会先用babel进行转码

    npm install babel-register --save-dev
使用方法如下：

    require('babel-require');
    require('example.js');
这个时候就不需要对example.js手动转码了，需要注意的是，babel-register只会对require命令加载的文件转码，而不会对当前文件转码。另外，由于它是实时转码，所以只适合在开发环境使用。

直接在代码中使用babel
-
代码中使用babel编译代码，需要安装babel-core模块

    npm i babel-core --save-dev
然后就可以在项目代码中直接使用了，使用方法：

    var babel = require('babel-core');
编译的主要方法是babel.transform与babel.transformFile方法。

babel.transform()方法：
```js
    babel.transform(code,options)
    //code:       js代码，一定要放在双引号里面，
    //options:    编译选项
```
transform方法返回的对象，里面有三个属性，分别是code、map、ast。

example:
```js
    var result = babel.transform("()=>1",options);
    //result.code:    表示编译后的代码
    //result.map:     表示编译后的sourceMap文件内容
```
    
babel.transformFile()方法：

```js
    babel.transformFile(filename,options,callback(err,result));
    //filename: 指定要被编译的文件
    //options： 编译选项
    //callback：编译完成后的回调函数，自带两个参数，分别是编译失败的错误
```
example:

```js
    babel.transformFile('example.js', options, function(err,result){
        result; //=>{code,map,ast}
        })
```
babel.transformFileSync()方法是transformFile的同步版本，用法也不同
```js
    babel.transformFileSync(filename,options)
```
这个方法同样返回包含三个属性的对象
```js
    var result = babel.transformFileSync('example.js', options);
    //=>{code, map, ast}
```
这几个方法的都可以写在JS文件中，直接使用node或者babel-node命令运行查看结果

options
-
上面的方法中都包含一个options参数，这个参数的具体内容请看[Babel官网API](http://babeljs.io/docs/core-packages/#options)

这些options同样适用于.babelrc文件，每一个的具体作用随后再翻译


























