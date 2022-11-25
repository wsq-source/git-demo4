# git指令
## 配置/修改
    name
        git config --global user.name "xxx"
    email
        git config --global user.email "xxx"

## 查看
    name
        git config user.name(vsdeveloper)
    email
        git cnfig user.email(1936539777@qq.com)
    git版本
        git -v
        git --version

## 使用git
    git status
        查看当前文件的状态, 是否被git所管理
        git中的文件有两种状态: 未跟踪(untracked)和已跟踪。
        未跟踪指文件没有被git所管理, 已跟踪指文件已被git管理。
        已跟踪的文件又有三种状态: 未修改、修改和暂存
        修改->暂存->入库->未修改
    git init
        初始化仓库
    git log
        查看当前版本及之前的commit记录
    
    刚刚添加到项目中的文件处于未跟踪状态
        未跟踪/修改 ==> 暂存(已跟踪)
            git add <filename>
                将文件切换到暂存的状态
            git add *
                将所有已修改(未跟踪)文件切换到暂存状态
        暂存 ==> 未修改
            git commit -m "xxx"
                将暂存的文件存储到仓库中(xxx是描述做了什么功能)
            git commit -a -m "xxx"
                将所有已修改的文件存储到仓库中(修改==>暂存==>未修改)
                未跟踪的文件不会提交
        未修改 ==> 修改(modified)
            修改代码后, 文件会变为修改状态

## 常用的命令
### 重置文件
    git restore <filename>
        重置(撤销)文件到最后一次提交后的状态
    git restore --staged <filename>
        把文件从暂存的状态取消
        vscode中绿色表示暂存状态, 红色表示未暂存状态
### 删除文件
    git rm <filename>
        删除文件(本地和git仓库)
    git rm <filename> -f
        强制删除文件(本地和git仓库)
### 移动文件
    git mv from to
        移动文件(重命名文件)


## 分支
    git在存储文件时, 每一次代码的提交都会创建一个与之对应的节点, git就是通过一个一个的节点来记录代码的状态的.
    节点会构成一个树状结构, 树状结构就意味着这个树会存在分支, 默认情况下仓库只有一个分支, 命名为master.
    在使用git时, 可以创建多个分支, 分支与分支之间相互独立, 在一个分支上修改代码不会影响其他的分支.

    git branch
        查看当前分支
    git branch <branch name>
        创建新的分支
    git branch -d <branch name>
        删除分支
    git switch <branch name>
        切换分支
    git switch -c <branch name>
        创建分支并将其设置为默认分支(创建并切换分支)
    
    在开发中, 都是在自己的分支上编写代码, 代码编写完成后, 在将自己的分支合并到主分支中

    git merge <branch name>
        合并分支(先切换到主分支, 再合并, 最后删除被合并的分支)

## 变基(rebase)
    在开发中除了通过merge来合并分支外, 还可以通过变基来完成分支的合并.

    我们通过merge合并分支时, 在提交记录中会将所有的分支创建和分支合并的过程全部都显示出来, 这样当项目比较复杂时, 开发过程比较波折时, 我们必须要反复的创建、合并、删除分支. 这样一来将会使得代码的提交记录变得极为混乱.

    原理(变基时发生了什么): 
        1.当我们发起变基时, git会首先找到两条分支最近的共同祖先
        2.对比当前分支相对于祖先的历史提交, 并且将它们的不同提取出来存储到一个临时文件中
        3.将当前部分指向目标的基底
        4.以当前基底开始, 重新执行历史操作

    操作:
        1.切换到要变基的分支
            git switch issue2
        2.输入变基指令指定目标分支
            git rebase master
        3.删除变基完成的分支
            git branch -d issue2

    rebase和merge对于合并分支来说最终的结果是一样的! 但是变基会使得代码的提交记录更整洁更清晰. 
    注意: 大部分情况下合并和变基是可以互换的, 但是如果分支已经提交给了远程仓库, 那么这时尽量不要使用变基.

## 远程仓库
    目前对于git所有操作都是本地进行的. 在开发中显然不能这样的, 这是我们就需要一个远程的git仓库. 远程的git仓库和本地的本质上没有什么区别.
    不同点在于远程的仓库可以被多人同时访问使用, 方便我们协同开发. 
    在实际工作中, git的服务器通常由公司搭建内部使用或是购买一些公共的私有git服务器. 
    学习阶段直接使用一些开放的公共git仓库. 目前常用的库有两个: GitHub和Gitee(码云).

### GitHub
    在远程库上创建一个库
        git remote add <remote name> <url>
    修改分支的名字为main
        git branch -M main
    将代码上传到服务器上
        git push -u <remote name> <local name>

    将本地库上传到git:
        git remote add origin https://github.com/wsq-source/git-demo.git
        git branch -M main
        git push -u origin main

### Gitee
    将本地库上传gitee
        git remote add gitee https://gitee.com/wangshuiqiang/git-demo.git
        git push -u gitee main