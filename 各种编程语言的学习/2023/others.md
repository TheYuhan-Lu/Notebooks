# 一些其他有的没的（为金鱼脑而设的记事本）
## git 
|**指令**|**用法及功能**|
|---|---|
|`git pull origin master`|从github上拉取文件,master对应的是分支名（github默认是main）|
|`git clone URL`|从github上下载URL所指向的库的文件|
|`git init`|在终端进入需要设置的本地文件夹，然后初始化概文件夹为库|
|`git add .`|将当前终端所在文件夹下的所有文件加入暂存区|
|`git commit -m "messages"`|commit暂存区至库（Repository）|
|`git remote add origin URL`|即连接URL指向的远程库|
|`git push -u origin master`|将本地Repo push至远程仓库中|
|`git remote set-url origin https://token@github.com/TheYuhan-Lu/reponame.git`|设置成ssh的方式链接远程库|


## hugo
|**指令**|**用法及功能**|
|---|---|
|`hugo new site name`|在当前所在目录建立以“name”为名字的文件夹，内含hugo框架的文件|
|` hugo serve --buildDrafts`|建立本地网站，可查看效果|
|`hugo --theme="hugo-theme-stack" --baseUrl="https://TheYuhan-Lu.github.io/" --buildDrafts`|在当前目录下创建public文件，建完之后将该文件夹push到git库即可|
|hugo new post/name.md|在post目录下新建一篇文章| 
