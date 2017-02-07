window.onload在页面中只能被执行一次
-
在模仿jquery实现的过程中，第一次采取了

```js
    function Iquery(fun){
        switch(typeof fun){
            case 'function':
                window.onload=fun
        }
    }
    Iquery(function(){
        alert(0)
    })                  //执行
    Iquery(function(){
        alert(1)
    })                  //不执行

    function add(n){
        switch(typeof n){
            case 'number':
                alert(n)
        }
    }
    add(1)              //执行
    add(1)              //执行
    function dev(fun){
        window.onload = fun;
    }
    dev(function(){alert(0)})   //执行
    dev(function(){alert(1)})   //不执行
```
的方法，发现alert事件只能被执行一次，这个时候，可以用window.addEventlistener来代替，为window添加事件是允许多个回调函数的