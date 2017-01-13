git add
-
提交文件至暂存区，

以前总是不懂什么叫暂存区，其实重点就两个字：暂存。git的版本控制系统分为暂存区和仓库两个部分，所有的修改可以先暂存，再提交到本地仓库中去

    git add example.js
git commit
-
把暂存区的文件提交到仓库中，也就是保存到本地仓库中，这个命令是有参数的

    git commit      //没有参数的时候，会进入一个编辑器，按照提示步骤输入当前提交的注释
    git commit -m   //后面可以直接跟注释内容
    example:
    git commit -m 'added file will be commit'
如果提交之后，发现有新的修改也想放到上次的提交里面去，可以使用--amend参数

    git commit -m 'add name';
    git commit --amend -m 'add name/email';
上面的命令执行后，会修改第一行的提交记录，这个修改包含了注释以及修改内容

git status 
-
查看git状态.

被修改(修改、增加、删除)的文件，会提示保存到暂存区，

已经保存到暂存区的文件，会提示保存到仓库中

git log 
-
查看提交历史，参数如下

命令后面可以直接添加分支名，显示其他分支的历史记录

git log默认的显示格式是这样的

    commit 0b7c644911fcaeeabc59b9c279fc4420549c1f78
    Merge: 0d88f35 854f884
    Author: chinaliyun <chinaliyun92@gmail.com>
    Date:   Thu Jan 5 10:15:06 2017 +0800

        fix bug
我们可以通过--oneline 来设置单行显示，如下所示：
    
    0b7c644 fix bug
如果git log后面跟有--pretty参数，会自动使用单行显示的格式

    --pretty 定义输出格式
    --pretty=format:'%h %ad %s %an'
    其中
        %h          缩写的hash值
        %d          是提交的装饰（如分支头或标签），
        %ad         提交日期
        %s          提交注释
        %an         提交作者
    --graph         以ASCII布局显示log
    --date=short    以最短的日期形式显示时间
输出标签的时候，如果是显示当前分支的历史记录，最新一条中除了会显示标签名，还会显示HEAD位置，如果是显示其他分支的历史记录也会显示当前历史记录所属的分支名

`git checkout <hash>/<branch>/<tag>`
-
1. 切换分支，或者切换到分支的指定状态(版本)
2. 用指定版本的文件覆盖本地文件

    git checkout <hash>         //切换到当前分支的指定版本
    git checkout <branch>       //切换到指定分支的最新版本
    git checkout <tag>          //根据标签名，切换到当前分支的指定版本
    git checkout -b  dev        
    //创建新的分支dev，并切换到dev分支,等同于：
    git branch dev
    git checkout dev
在切换分支的时候，如果当前分支存在需要add或者commit的修改，会收到提示

我感觉分支的意思和“沙箱”是差不多的，沙箱中的内容不会影响主要分支的内容，如果沙箱中的运行结果满足了任务的需要，就可以把沙箱的修改合并到主要分支就可以了

如果再修改的过程中，本地文件已经被修改且没有被提交到暂存区，可以使用`git checkout example.js`的方式恢复内容到仓库最新版本

git tag
-
列出当前项目中所有的标签，如果后面添加了新的标签名，就是为上一个提交点指定一个标签，可以在gti checkout 的时候作为参数使用。这些标签在当前项目的任何一个分支上都可以看得到。

    -d  后跟标签名，可以删除指定标签

git branch
-
列出当前项目的所有分支名称

如果后面添加了新的分支名称，则表示创建一个新的分支

    -d  删除指定分支
    -D  强制删除指定分支，当分支内存在未commit的修改，又要舍弃这些修改的时候，需要用这个参数

如果添加了--track 参数 则是表示新建的分支跟踪远程仓库的某个分支，比如

    git branch --track dev oringin/dev
就表示新建本地分支dev，并让他跟踪origin上dev分支

git merge
-
合并分支，主要用于把指定分支合并到当前分支

这里有两种情况，一种会出现合并冲突，另一种不会

        vi hello.txt
        git add hello.txt
        git commit -m 'edit before new branch'
        git checkouot -b dev
        vi hello.txt
        git add hello.txt
        git commit -m 'edit in dev'
        git checkout master
        git merge dev
上面分支后，原分支没有任何修改，不会出现合并冲突
    
        git checkouot -b dev
        vi hello.txt
        git add hello.txt
        git commit -m 'edit in dev'
        git checkout master
        vi hello.txt
        git add hello.txt
        git commit -m 'edit before new branch'
        git merge dev
上面的代码就会出现合并冲突，需要解决这个冲突才能继续，注意上面代码的顺序是不一样的。

这里注意一个事情，就是git的merge不是真的代码合并，而是把当前的分支的指针，指向指定的分支，所以被称为快速合并

如果出现了合并冲突，找到文件中提示的冲突位置，修改后重新提交即可

所以在项目中我们可以按照以下流程使用

1. 为了测试一个功能，创建一个新分支
2. 新分支内容可以使用，直接commit
3. 把新分支合并到master分支，
4. 如果出现冲突，修改提示的冲突内容
5. 冲突内容修改之后，重新add commit，这个时候master是我们想要的内容，而分支的内容是不对的，内容和master不一样
6. 这个时候要么直接删除新分支，要么进入新分支重新把master分支合并到新分支，这样就可以保证新分支的内容和master保持一致，可以继续在新分支上增加新功能，然后重复上面的操作

`git reset HEAD`
-
撤销上次的暂存修改，但不会更改本地文件

如果存在未提交的修改，会先提示提交修改

如果想把本地文件的修改也撤销，可以使用git checkout

`git reset <hash><tag>`
-
指定一条提交记录，这条记录之后的记录，都不会在当前分支的记录上显示。如果这些记录有tag，他们就仍然保存在仓库中，便于引用，可以使用git log --all命令查看到这些记录

不在分支上显示的带有tag的记录，在使用git tag -d命令删除标签后，**这条记录以及它前面的没有tag的记录***会被当做垃圾回收

比较一下两个命令的不同结果：

    * b300980  2017-01-05 | 04th | ( (tag: v4)) | [chinaliyun]
    * ae5361f  2017-01-05 | 03th | () | [chinaliyun]
    * 1c17e77  2017-01-05 | 02th | ( (tag: v2)) | [chinaliyun]
    * 2ce0525  2017-01-05 | 01th | ( (HEAD -> master)) | [chinaliyun]
删除v2标签：

    git tag -d v2
    git hist --all

    * b300980  2017-01-05 | 04th | ( (tag: v4)) | [chinaliyun]
    * ae5361f  2017-01-05 | 03th | () | [chinaliyun]
    * 2ce0525  2017-01-05 | 01th | ( (HEAD -> master)) | [chinaliyun]
删除v4标签：
    
    git tag -d v4
    git hist --all
    * 1c17e77  2017-01-05 | 02th | ( (tag: v2)) | [chinaliyun]
    * 2ce0525  2017-01-05 | 01th | ( (HEAD -> master)) | [chinaliyun]
注意上面的两个操作是为了比较删除不同标签的结果，不是先删除v2，再删除v4的结果

`git revert HEAD`
撤销仓库中的最后一次提交操作，本地文件修改的内容也会被撤销，如果存在未commit的修改，会收到提示

我自己试了一下能不能撤销其他的提交操作  ，就直接提示错误了，这个时候不管什么操作 ，都会暴露一个currently revert的错误，必须使用revert --abort终止撤销操作

git mv
-
git中的移动文件，这个命令的结果会被直接保存到暂存区，直接使用git commit提交即可，例如：

    git mv example.js lib\
和下面命令的结果是相同的

    mv example lib\         //移动文件到新的文件夹
    git add lib\example.js  //提交修改
    git rm example.js       //这个命令在删除文件的同时，也会自动把修改提交到暂存区

git clone
-
克隆一个仓库到新的文件夹

    mkdir hello
    git init hello
    git clone hello hello_clone
这里就直接创建了一个新的文件夹hello_clone，并且已经把hello的git仓库克隆了过来，这个时候，hello仓库就是hello_clone仓库的远程仓库，

这个克隆过程中，只克隆master分支的记录，其他分支的记录，新仓库会添加一个引用，便于我们再新仓库创建分支后，从远程仓库的记录上拷贝分支的文件，这里的拷贝使用git merge方法

    git checkout -b dev
    git merge origin/dev
这样就可以吧远程仓库dev分支的文件合并到当前仓库的dev分支

git fetch
-
获取远程仓库的最新提交记录，注意只是获取提交记录了，不会直接修改本地文件，而且这条记录在本地分支上不会显示的，需要使用git log --all命令才能看到，而且可以明显的看到 这条记录是存在远程仓库上的，如果需要合并记录中的文件到当前仓库的分支上，需要用git merge命令

git pull
-
这个命令等价于git fetch 和 git merge，也就是说，他会自动查询指定的远程仓库，并合并到本地仓库
