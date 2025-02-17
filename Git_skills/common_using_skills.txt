一、上传文件：

    //仓库初始化
    git init

    //添加变更到暂存区
    git add README.md   //添加文件README.md
    git add .   //一次性添加所有变更

    //提交
    git commit -m "first commit"

    //连接到远程仓库
    git remote add origin git@github.com:zhouwenbin888/ceshi.git

    //推送到github
    git push -u origin master



二、版本回退：

    //撤消上次提交并返回到上一个提交（使HEAD指向上一个提交，但不会删除您最新的更改）
    git reset HEAD~1

    //完全返回到以前的提交并放弃所有更改
    git reset --hard HEAD~1


三·、其他常用指令：

    git status：查看当前工作区和暂存区的状态。
    git log：查看提交记录。
    git branch：管理分支。
    git remote：管理远程仓库。