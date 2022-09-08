# Git

git是一种源码管理系统（source code management，缩写为SCM）。它对当前文件提供版本管理功能，核心思想是对当前文件建立一个对象数据库（object database），将历史版本信息存放在这个数据库中。


## git 操作流程

- 安装 git
	执行 `git --version` 命令，如果显示 git 的版本就说明安装成功。
- 提交用户名和电子邮件
	这些信息将作为提交者信息显示在更新历史中。

```sh
git config --global user.name 'you'
git config --global user.email 'you@gmail.com'
```

git 的设定被存放在用户本地目录的 .gitconfig 文件里，也可以直接编辑该配置文件。


1. 新建一个 git 仓库

```sh
git init
```


2. 查看目前状态

```sh
git status
```

添加-s选项，就可以不显示讲解。如果再添加-b选项，就不显示讲解，但显示分支的名称。


3. 添加文件从工作区到暂存区

```sh
git add <file name>
# or
git add .
```

指定参数 `.` 可以将所有文件加入到暂存区。
添加 `-u` 选项，就只添加已提交过的文件到索引。
添加 `-p` 选项，就可以只添加文件修改的其中一部分。 
添加 `-i` 选项，可以选择用对话形式显示添加在索引的文件。


4. 从暂存区提交到本地代码仓库

```sh
git commit -m '提交信息'
```

添加 `-a` 选项，就可以检测出修改的文件 (不包括新添加的文件)，将其添加至索引并提交。
添加 `-m` 选项，就可以指令提交 “提交信息”。如果不添加 `-m` 选项，就会启动修改提交信息的编辑器。


5. 查看提交 commit 的信息

```sh
git log
# or
git log --graph --oneline
```

若要查看特定文件的提交记录，请指定文件名称。
添加 `--decorate` 选项执行，可以显示包含标签资料的历史记录。


6. 添加远程指针

```sh
git remote add <name> <url>
```

在 `<name>` 处输入远程数据库名称，在 `<url>` 处指定远程数据库的URL。

执行推送或者拉取的时候，如果省略了远程数据库的名称，则默认使用名为”origin“的远程数据库。因此一般都会把远程数据库命名为origin。


7. 将本机的 master 分支推送到远程 origin 主机。

```sh
git push -u origin master
```

`-u` 参数表示记住对应关系，下次可以直接 `git push` 推送。


8. 将远程主机 origin 的代码取回本地，与本地的 master 分支合并

```sh
git pull origin master
```


9. 查看与上一次 commit 的区别

```sh
git diff HEAD
```

不指定选项可以显示工作树和索引的差异。添加－cached 选项可以显示索引与HEAD的差异。如果指定HEAD或提交，则可以显示工作树和指定HEAD之间的差异。


## 远程数据库操作

1. 复制现有的远程数据库

```sh
git clone <repository> <directory>
```

在 `<repository>` 处指定远程数据库的 url，在 `<directory>` 处指定新目录的名称。
执行复制 clone 命令就会自动设定为追踪远程数据库。在之后的 push/fetch/pull 命令时，即使忽略 repository，也可以正确地显示、读取修改内容。

2. 显示远程数据库列表

```sh
git remote
```

3. 复制现有的远程数据库

```sh
git remote add <name> <url>
```

4. 在远程数据库的分支创建本地数据库的分支

```sh
git checkout <branch>
```

用chekout命令参数指定远程数据库的分支，就可以通过远程数据库的分支在本地数据库创建分支。
如果 git 版本太旧而不能创建，可以使用下面的方法在 branch 命令创建分支。

```sh
git branch <branchname> origin/<branch>
```


5. 在远程数据库创建分支 / 反映修改内容到分支

```sh
git push <repository> <refspec>
```

在 `<repository>` 处指定远程数据库的 url 或远程数据库名称（origin。
在 `<refspec>` 指定分支名称。如果省略，远程数据库和本地数据库所有的分支会默认被列为目标。
添加 `-u` 选项可以追踪在远程数据库的目标分支。


6. 查看远程数据库分支的修改内容

```sh
git fetch <repository> <refspec>
```

7. 读取远程数据库的分支的修改内容

```sh
git push <repository> <refspec>
```

tip：pull = fetch + merge.


8. 删除远程数据库分支

```sh
git push --delete <repository> :<branchname>
```

1.7 以前的 git 版本没有 `--delete` 选项（下同）。


9. 建立远程数据库的标签

```sh
git push <repository> <tagname>
```

添加 `-tags` 选项，可以把本地数据库里所有的标签添加到远程数据库。


10. 删除远程数据库的标签

```sh
git remote --dalete <repository> <tagname>
```


11. 修改已注册的远程数据库的电子邮件地址

```sh
git remote set-url <name> <url>
```

12. 修改以注册的远程数据库

```sh
git remote rename <old> <new>
```

在 `<old>` 把已指定名称注册的远程数据库的名称改为 `<new>` 。


## 发布一个版本

为当前分支打上版本号。

```sh
git tag -a [VERSION] -m 'released [VERSION]'
git push origin [VERSION]
```


## 分支

分支是为了将修改记录的整体流程分叉保存。分叉后的分支不受其他分支的影响，所以在同一个数据库里可以同时进行多个修改。

![[1.png]]

当功能分支或修复 bug 分支写好后可以合并分支到主分支中。

- master 分支。在数据库进行最初的提交后, Git会创建一个名为 master 的分支（如果是在GitHub创建的仓库会是 main 的分支）。因此之后的提交，在切换分支之前都会添加到 master 分支里。

- Merge 分支是为了可以随时发布 release 而创建的分支，它还能作为 Topic 分支的源分支使用。master 分支通常当作 merge 分支。

- Topic 分支是为了开发新功能或修复Bug等任务而建立的分支。Topic分支是从稳定的Merge分支创建的。

### 分支操作

1. 操作分支

```sh
git branch <branchname>
```

添加 `-a` 选项，就可以显示包括远端分支在内的分支清单。
添加 `-m <oldbranch> <newbranch>` 修改分支名称。
添加 `-d` 可以删除指定名称的分支。
如果不指定任何参数，直接执行branch命令会显示分支列表。

2. 切换分支

```sh
git checkout <branchname>
```

添加 `-b` 选项，可以新建分支并切换到该分支。

3. 合并分支

```sh
git merge <branchname>
```

合并指定分支到当前分支。

添加 `--no-ff` 选项，就是fast-forward合并可以建立合并提交，即创建一个新的提交来记录合并。


### 分支策略

![[2.jpg]]


## 标签

Git可以使用2种标签：轻标签和注解标签。打上的标签是固定的，不能像分支那样可以移动位置。
一般情况下，发布标签是采用注解标签来添加注解或签名的。轻标签是为了在本地暂时使用或一次性使用。


操作标签：

```sh
git tag <tagname>
```

添加 `-a` 建立指定含注解的标签。执行后会启动编辑区，请输入注解，也可以指定-m选项来添加注解。
添加 `-n` 可以显示标签的列表和注解。
添加 `-d` 可以删除指定标签。


## 操作提交记录

1. 修改最近的记录

```sh
git commit --amend -m [message]
```

添加 `--amend` 选项，可以修改同一个分支最近的提交内容和注解。


2. 修改过去的提交记录

```sh
git rebase -i <commit>
```

如果指定提交之后再次指定提交，就会显示提交清单。
请在清单里找出要修改的提交，将该行的 "pick" 改成 "edit"，之后保存并退出。

接着，编辑要修改的文件，保存文件之后指定 `--amend` 选项，以执行提交。

```sh
git commit --amend
```

最后，指定 `--continue` 选项以执行 rebase。

```sh
git rebase --continue
```


3. 中途停止 rebase

```sh
git rebase --abort
```

指定 `--abort` 选项并执行 rebase 命令后，可以取消 rebase。


4. 查看 HEAD 的移动历史

```sh
git reflog
```

添加参数 `<ref>` 指定分支名称，可以查看分支前面的移动历史记录。


5. 放弃最近的提交

```sh
git reset --hard HEAD~
```

添加 `--hard` 选项，会重置当前工作区与 commit 保持一致。
添加 `--keep` 选项，可以保持暂存区和工作区不变。


6. 放弃 rebase

```sh
git reset --hard <commit>
```

在 `<commit>` 处填写 确认提交的信号值或「HEAD@{数字}」值，可以使用 `git reflog` 命令查看。

7. 取消最近的 reset 

```sh
git reset --hard ORIG_HEAD
```

ORIG_HEAD 是指 reset 之前的提交，指定之后reset。

8. 复制提交

```sh
git cherry-pick "<commit>"
```

把指定 `<commit>` 的提交复制到现在的分支。

9. 查找包含特定注解的提交

```sh
git log --grep "pattern"
```

只显示提交记录里包含指定 `<pattern>` 文字的提交。
