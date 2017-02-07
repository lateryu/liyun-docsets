为什么函数参数为function的时候，只能执行一次
```js
        function Iquery(fun){
            switch(typeof fun){
                case 'function':
                    window.onload=fun
            }
        }
        Iquery(function(){
            alert(0)
        })  //执行
        Iquery(function(){
            alert(1)
        })  //不执行
    
        function add(n){
            switch(typeof n){
                case 'number':
                    alert(n)
            }
        }
        add(1)      //执行
        add(1)      //执行
        function dev(fun){
            window.onload = fun;
        }
        dev(function(){alert(0)})   //执行
        dev(function(){alert(1)})   //不执行
````