>学习了React很久，始终摸不着门路，这次重新开始学习，发现了一些问题，记录下来。

容易写错的地方
-
1. ReactDOM，一定要注意DOM全部是大写的，有一个小写都报错
2. 不管是html内部的JSX，还是引入外部的JSX文件，一定要记住script标签里面的type属性一定是text/babel。之前有一个写法是text/jsx，是因为在browser.js之前有一个JSXtransformer.js的库，也是为了转换JSX语法的，他需要脚本的type写成text/jsx， 后来升级为browser.js后，就要写成text/babel了；
3. 

react-tools
-
这个插件使用来编译JSX文件的，可以直接使用npm安装

    npm i react-tools -g
    jsx -w src/ build/
这里要注意的是，命令行中使用jsx命令，后面必须要跟两个文件夹，他会吧第一个文件夹内的JSX文件，转成JavaScript语法，例如：

 //SX转译前

        ReactDOM.render(
            <h2>Hello world</h2>, 
            document.getElementById('example')
        )
//JSX转译后

        ReactDOM.render(
            React.createElement("h2", null, "Hello world"), 
            document.getElementById('example')
        )

使用react的网站源码连的三个JS文件
-
一共用了三个库： react.js 、react-dom.js 和 Browser.js ，它们必须首先加载。其中，
```
- react.js      是 React 的核心库，
- react-dom.js  是提供与 DOM 相关的功能，
- Browser.js    的作用是将 JSX 语法转为 JavaScript 语法，这一步很消耗时间，实际上线的时候，应该将它放到服务器完成。
```
之前看过阮一峰的相关教程，引入browser.js包的时候，用的npm安装的browser包，结果就一直报错，研究之后才发现，此包非彼包啊，npm上的browser包和browser.js完全就是两码事，所以这个坑要提醒一下队友们

那么这个browser.js是干嘛用的？因为React独有的JSC语法和JavaScript不兼容，所以所有使用JSX的地方，都要引入这个库把JSX转成JavaScript语法，这个步骤上线的时候最好是放到服务器上完成，因为它太消耗时间了

React组件类注意事项：
-
- 组件名称首字母必须大写
- 组件内只能包含一个顶层元素(顶层标签)
- 组件类的属性在this.props上获取
- class、for是JS的保留字，所以这两个属性要写成className和htmlFor

this.props.children
-
this.props.children 的值有三种可能：如果当前组件没有子节点，它就是 undefined ;如果有一个子节点，数据类型是 object ；如果有多个子节点，数据类型就是 array 。所以，处理 this.props.children 的时候要小心。


prop相关的方法
-

获取属性的值：
    
    this.props.name
设置默认属性值：
    
    getDefaultProps: function(){return {
        name:value
    }}
检查数据类型：
    
    propTypes: function(){return {
        name: fn
    }}

refs 相关方法
-
获取指定的DOM节点

    this.refs.name
state 相关方法
-
获取指定状态
    
    this.state.name
设置指定状态
    
    this.setState({name:value})
设置默认状态
    
    getInitialState(function(){
        return {name:value,name:value,..}
    })    
react中的表单
-
用户在表单填入的内容，属于用户跟组件的互动，所以不能用 this.props 读取；文本输入框的值，不能用 this.props.value 读取，而要定义一个 onChange 事件的回调函数，通过 event.target.value 读取用户输入的值。textarea 元素、select元素、radio元素都属于这种情况
属性
-
获取某一个属性值

    render: function(){
        return <h1> {this.props.title} </h1>
    }
设置默认属性值：
    
    getDefaultProps: function(){
        return {
            title: 'hello world'
        }
    },
检查属性值类型：
    
    PropTypes: {
        title: React.PropTypes.string.isRequired,
    }


获取真实的DOM节点
-
获取当前组件内的某一个DOM节点
    
    render: function(){
        return <h1 ref="myTextRef"> {this.refs.myTextRef.nodeName} </h1>
    }
这里的this指的是 当前的组件，refs是当前组件里面所有有ref属性的集合

设置组件状态
-
获取某个状态的值
    
    render: function(){
        return <div>{this.state.name}</div>
    }
设置状态的默认值：
    
    getInitialState: function(){
        return {
            name: value
        };
    }
设置/修改某个状态值：
    
    this.setState({namne:value,name:value,...})

react中通过ajax获取内容
-
在react中使用ajax的时候，要注意ajax方法里面的this，比如success,error方法中的this一定要提前绑定，可以提前使用_this =this，或者bind(this)的方式，否则会因为this的作用与问题出错。

ajax里面的success\error等方法，也不要简写为success(){}.bind(this),这样也是会报错的












