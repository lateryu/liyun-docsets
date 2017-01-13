.gitignore 配置文件用于配置不需要加入版本管理的文件，配置好该文件可以为我们的版本管理带来很大的便利，以下是个人对于配置 .gitignore 的一些心得。

语法
-
以斜杠“/”开头表示目录；

以星号“*”通配多个字符；

以问号“?”通配单个字符

以方括号“[]”包含单个字符的匹配列表；

以叹号“!”表示不忽略(跟踪)匹配到的文件或目录；

实例
-

    /*  
    /.ignore
    /application
上面的配置会先忽略目录下的所有文件，然后声明不忽略的.ignore文件和application文件夹

    /node_module/
上面语句会声明忽略根目录下的node_module文件夹

    node_module/
上面语句就不同了，他会忽略目录下所有的node_module文件夹，不管在哪个文件层里面

    /node_module/*
上面语句会忽略node_module下面的所有文件，但不会忽略node_module文件夹本身