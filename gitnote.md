# Git笔记
## 登录Git
 1.git config --global user.email "you@example.com"
 2.git config --global user.name "Your Name"

## 创建版本库：
### 1.在所选目录下创建文件夹
### 2.空白处右键选择Git Bush Here 将Git文件目录设置成当前目录
### 3.输入git init命令创建版本库（创建唯一master分支）

## 文件的上传
### 1.编译文件并保存在learngit文件夹下（工作区）
### 2.将文件添加到仓库（add-暂存区）(git add readme.txt)
### 3.提交到仓库（更新分支）(git commit -m "wrote a readme file")
    注：-m后“”内的内容可以自己编译，最好是有意义的
### 4.命令git commit执行成功后：
    1 file changed1 一个文件被改动
    2 insertionsv 插入了两行内容
### 5.git commit只上传暂存区内的文件，不上传工作区的文件
### 6.为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：
    $ git add file1.txt
    $ git add file2.txt file3.txt
    $ git commit -m "add 3 files."

## 文件的修改：
### 1.对工作区的文件进行修改
### 2.运行git status命令可掌握仓库当前的状态
例如：
    1）哪些文件被修改过
    2）如果多个文件被修改只有其中的几个重新上传成功则会显示未上传成功的内容
    3）被修改项目若成功add暂存库 modified:   xxx.xx
    会显示绿色，反之为红色
    4）若修改的项目全部上传成功则显示nothing to commit, working tree clean
### 3.运行git diff命令可查看上次的修改内容是什么(将工作区与仓库里的文件进行对比)

## 时光机：
### 1.运行git log命令可查看历史提交版本（可以使用git log --pretty=oneline查看版本的简化版，开头一串为版本号）
### 2.执行cat XXX.XX命令可查看XXX文件的当前版本
### 3.执行git reset --hard head^命令回退到上一个版本（在Git中，用head表示当前版本，上一个版本就是head^，上上一个版本就是head^^，当然往上100个版本写100个^比较容易数不过来，所以写成head~100）
### 4.执行 git reset --hard 版本号,可回到所选版本
### 5.执行git reflog可查看历史操作（便于查找历史）
### 6.撤销修改：
1)将工作区的文件进行修改（git checkout -- xxx.xx）
    一种是xxx.xx自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
    一种是xxx.xx已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
2)将暂存区的文件进行回退至工作区(git reset head xxx.xx)
### 7.删除文件：
    1)rm xxx.xx从工作区里删除
    2)git checkout -- xxx.xx将工作区误删的文件从版本库中恢复
    3)从版本库中删除：
        (1)git rm xxx.xx
        (2)git commit -m "xxx.xx"

## 本地上传至远程库(默认名字origin)
### 1.关联：git remote add origin git@github.com:用户名/本地库文件夹名称.git
### 2.本地推送至远程库：
    1)第一次推git push -u origin master
    2)git push origin master

##分支管理
###1.创建分支与合并
    1）创建加切换分支（dev）git checkout -b dev
    (git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
        $ git branch dev(创建分支)
        $ git checkout dev(切换分支)
    )
    2）查看当前分支git branch（绿色为当前分支）
    3)合并分支git merge
        git merge dev命令用于合并指定分支（dev）到当前分支。
    4）删除分支（dev）git branch -d dev
        删除多个分支（a\b\c）git branch -d a b c
    5）强行删除分支（dev）git branch -D dev
###2.解决冲突
    1）当两个分支同时修改一处地方，合并发成冲突的时候，可以在本地工作区对文件冲突的地方（Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容）进行手动修改后再重新进行上传
    2)查看分支合并的情况：
    （1）git log --graph（详细内容）
    （2）git log --graph --pretty=oneline（简化）
    （3）git log --graph --pretty=oneline --abbrev-commit（最简化）
###3.分支管理策略
    1）一般地，分支合并时使用的是fast forward模式，但是这种模式合并分支后会丢掉合并的分支信息，所以不便于查找历史。如果想要强制禁用fast forward模式，就需要在merge的时候重新生成一个commit，这样从分支历史上就能看到分支信息。
        {git merge --no-ff -m "merge with no-ff" dev（合并分支并且禁用fast forward；重新创建一个-m）}
    2）一般地，master分支应该是非常稳点的（主分支），不用做编译。需要创建一个新的分支（dev）来进行编译(可以在dev上创建多个分支进行编译以及合并)。等dev确认编译完成后再与master进行合并。
###4.BUG分支
    当手中dev分支上的任务正在进行时，中途需要对其他项目进行操作（修改BUG），手中的dev分支任务无法进行存储合并：
    1）可以先使用“git stash”将dev的工作现场储藏起来；
    2)转移至需要工作的分支，例如master分支需要进行BUG修复：
        （1）转移至master分支
        （2）创建新的临时分支
        （3）对分支进行操作后进行合并，删除临时分支
    3）回到dev分支，执行git stash apply恢复后再执行git stash drop来删除stash的内容（或直接使用git stash pop可直接恢复并且删除stash内容）
    4）git stash list可以查看stash中的内容
###5.多人协作
    1）查看远程仓库（命令：git remote/git remote -v）(fetch为抓取地址即克隆；push为推送地址）
    2）推送分支至对应分支上（例如dev）git push origin dev
    3)从远程仓库克隆的时候默认情况下只能看到本地的master分支，需要创建远程origin的dev分支到本地（创建的分支名称最好与远程分支的一致）：
        git checkout -b dev origin/dev
    这样就能在dev上进行修改并把dev分支push到远程
    4）当两个或者多个人对同一分支的同一处进行了修改并上传时会提示发生冲突且推送失败。（远程库上这一部分已经发生了变化）需要使用git pull将最新的提交抓取下来，在本地进行修改解决冲突后再进行推送。
    5）如何设置本地分支与远程分支的链接：
        (1)git branch --set-upstream-to=origin/branch-name branch-name
        (2)git branch --set-upstream-to branch-name origin/branch-name
        (3)git branch --set-upstream branch-name origin/branch-name
    6）多人协作的工作模式通常是这样：
        （1）首先，可以试图用git push origin <branch-name>推送自己的修改；
        （2）如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
        （3）如果合并有冲突，则解决冲突，并在本地提交；
        （4）没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。
    7）Rebase
        （1）rebase操作可以把本地未push的分叉提交历史整理成直线；
        （2）rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。


