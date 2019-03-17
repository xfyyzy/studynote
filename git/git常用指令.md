[TOC]



# git常用指令

## 1.基本操作：

1. `git init`初始化git仓库；

2. `git add`添加文件到暂存区；

3. `git commit -m "message"`提交到git仓库；

4. `git status`查看git仓库状态；

5. `git diff`查看修改，`git diff HEAD`查看工作区文件与版本库最新版本的差别；

6. `git log`查看历史记录；

    |       参数       |        用法        |
    | :--------------: | :----------------: |
    | --pretty=oneline | 以一行显示简要信息 |

## 2. 回退操作：

1. `HEAD`表示当前版本，`HEAD^`表示上一个版本，`HEAD^^`表示上上个版本，`HEAD~num`往上num个版本；
2. `git reset`回退指令，`git reset --hard HEAD^`回退到上一个版本；
3. `git reset --hard <commit id>`回退到指定commit id，commit id 只需前几位即可；
4. `HEAD`相当于头指针，指向当前分支的当前版本；
5. `git reflog`查看过往命令；

## 3. 工作区，暂存区，版本库：

1. 工作区即文件夹；
2. `git add`添加修改至暂存区；
3. `git commit `添加修改至版本库；
4. `.git`文件夹存储暂存区与版本库数据；
5. 文件先被提交至暂存区，再被提交至版本库；

## 4.撤销工作区修改

1. `git checkout -- filename`使工作区文件恢复到最近一次`git add`(添加到暂存区未提交前又修改了)
    或者`git commit`;
2. `git checkout`如果没有加`--`则是切换分支的命令；

## 5.撤销暂存区修改

1. 适用于错误的修改已经提交至暂存区；
2. `git reset HEAD filename`把暂存区修改撤销，变成工作区已修改；

## 6.删除文件

1. 使用`git rm`从版本库删除文件；

## 7.远程仓库

1. 创建SSH-Key:`ssh-keygen -t rsa -C "youemail@example"`；
2. 主目录下的`.ssh`文件夹，`id_rsa`为私钥，`id_rsa.pub`为公钥；
3. 私钥不可泄漏；

### 7.1关联远程仓库

1. **创建github仓库**；
2. `git remote add origin git@github.com:xfyyzy/javaHomeWork.git`,关联远程仓库。关联后，origin就是远程仓库的名字。
3. 如果需同时关联多个远程仓库，把"origin"换成不同的两个名字就好；

### 7.2推送至远程仓库

1. `git push origin master`推送**master**分支至远程仓库；`git push origin dev`推送**dev**分支至远程仓库；

| 参数 |          用法          |
| :--: | :--------------------: |
|  -u  | 关联本地分支与远程分支 |

### 7.3拉取远程仓库

1. `git branch --set-upstream-to=origin/dev dev`设置`origin/dev`与`dev`的链接，不然拉取会失败；

### 7.4克隆远程仓库

1. `git clone`;
2. 只有克隆自己的远程仓库才可以提交修改；

### 7.5查看远程仓库信息

1. `git remote -v`;

### 7.6删除远程仓库

1. `git remote rm origin`;

## 8.分支管理

1. `master`与`dev`分别是分支指针，`HEAD`指向的指针为当前版本。创建、合并、删除分支其实就是指针的操作；

    **2. 每个分支拥有独立的工作区，暂存区，版本库；**
    **3.新创建的分支会复制母分支的三个区域；**
    **4.分支提交后，母分支工作区也变干净，改变被提交至子分支；**
    **5.当分支有修改未被提交时，无法切换至除新建以外的其他分支；**

### 8.1创建与切换分支

1. `git branch dev`创建`dev`分支；
2. `git checkout dev`切换至`dev`分支；
3. `git checkout -b dev`创建并切换至`dev`分支；

### 8.2查看分支状态

1. `git banch`查看分支状态，当前分支会用*号标记；

### 8.3合并分支

1. `git merge`合并指定分支到当前分支，如`git merge dev`合并dev至当前分支；
2. `Fast-forword`快进模式，当两个分支的修改没有冲突时；

### 8.4删除分支

1. `git branch -d dev`删除dev分支；
2. `git branch -D dev`强制删除未merge的分支；

### 8.5解决冲突

1. 当尝试merge冲突的文件时，git会将冲突的部分标记在文件中，直到在一条分支解决冲突；
2. `git log --graph`可以查看分支合并图；

### 8.6分支管理策略

1. merge时git会自动尝试使用`Fast Forward`模式，该模式在合并并删除分支后会丢失分支信息；使用

    `--no-ff`参数禁用`Fast Forward`，在merge时生成一个新的commit,可以在分支历史上看到分支信息。

    eg:`git merge --no-ff -m "message" dev`。

2. 团队合作时，成员各自工作，然后合并至dev,dev再合并至master。

3. `master`分支`dev`分支需要与远程仓库同步；

4. `bug`分支不需要与远程仓库同步；

5. `feature`分支同步与否要看是否远程合作开发；

### 8.7抓取分支

1. `git clone`时，默认只能看到master分支；
2. `git checkout -b dev origin/dev`在**`git clone`以后**创建远程dev分支到本地；

## 9.保存工作现场

1. 使用`git stash`保存工作现场，可以在不提交的情况下切换至其他分支；
2. `git stash list`查看已保存的工作现场；
3. `git stash apply`恢复工作现场但不删除，`git stash drop`删除stash,`git stash pop`恢复并删除stash;

## 10.变基

1. `git rebase`将两条分支的不同修改变成补丁应用在其中一条分支上,使提交历史成为一条直线；
2. 变基会丢弃一些以前的提交，不要在公共仓库上使用；

## 11.标签管理

1. 标签是指向某个commit的指针；
### 11.1创建标签
1. `git tag <tagName> <commit id>`默认给当前最新的commit打标签；
2. `git tag`查看已打标签，标签不按时间列出，而按字母排序；
3. `git show <tagName>`查看标签信息；
4. `git tag -a <tagName> -m "This is the fist edition" <commit id>`使用`-m`参数指定说明文字；
### 11.2删除标签
1. `git tag -d <tagName>`使用`-d`参数删除标签;
### 11.3远程标签
1. `git push origin <tagName>`推送标签至远程；
2. `git push origin --tags`一次性推送所有标签至远程；

#### 11.3.1删除远程标签

1. 先删除本地标签；
2. `git push origin :refs/tags/tagName`远程删除名为”tagName“的标签；

## 12.自定义Git
### 12.1忽略特殊文件
1. 在git 工作区新建一个`.gitignore`文件，填入需要忽略的文件；
2. github有已经整理好的`.gitignore`文件；
3. `git add -f filename`使用`-f`参数强制添加被忽略的文件；
4. `git check-ignore`检查`.gitignore`文件,e.g:`git check-ignore -v App.class`;

### 12.2配置文件

1. 本仓库的配置文件在`.git/config`文件中，只对本仓库起作用；
2. 当前用户的配置文件在`~/.gitconfig`文件中，全局生效；
3. 查看当前配置信息`git config <--global/--local> --list`





