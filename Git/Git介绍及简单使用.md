- 一个云代码托管工具

[Git](https://git-scm.com/book/zh/v2)
## 常用命令

- 初次使用，配置你的用户名及邮箱

```console
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```
这里加了global, 是全局配置

- 初始化本地仓库
```
git init
```
生成一个.git文件。该仓库可以被自主识别为git仓库

- 标记，跟踪一个文件
```
git add <file>
```
- 生成版本
```
git commit -m "更新描述"
```
- 同步到云端
```
git push
```
此时会生成一个与项目同名称的库，push时可以选择私有或公开
- 克隆现有的仓库
```console
$ git clone https://github.com/libgit2/libgit2
```
此时远程拉取了一个libgit2的库，包含.git文件夹
- 克隆时自命名仓库名
```console
$ git clone https://github.com/libgit2/libgit2 mylibgit
```
将克隆过来的库自命名为mylibgit

以上使用http协议，也可使用，或下载zip文件

## 记录更新

- 查看文件状态，已跟踪未跟踪等
```
git status
```
## 忽略一些文件，不纳入跟踪文件列表
- 创建.gitignore文件
```console
$ cat .gitignore
*.[oa]
*~
```
第一行告诉 Git 忽略所有以 `.o` 或 `.a` 结尾的文件。一般这类对象文件和存档文件都是编译过程中出现的。 第二行告诉 Git 忽略所有名字以波浪符（~）结尾的文件。

一个.gitnore文件例程
```
# 忽略所有的 .a 文件
*.a

# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a

# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下名为 build 的文件夹
build/

# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```
## 分支与合并

- 默认只有一条主分支main
- 创建分支后可以在不影响主分支的情况下进行开发
- 创建分支
```console
$ git branch testing
```
- 分支切换
```console
$ git checkout testing
```
- 分支的合并
- 分支创建时以创建分支时的环境为样本
现在创建了iss53和hotfix两个分支
![[Pasted image 20260329164849.png]]
其中hotfix分支的处理是有效的，将其合并到主分支
- 先切换到主分支。(现在的主分支名称为main, 图片来自git官方文档)
```
git checkout main
```
- 合并hotfix
```
git merge hotfix
```
这里是快速合并，只需要移动指针

此时可以删除掉不需要的分支，因为hotfix与main已指向同一个地方
```console
git branch -d hotfix
```

![[Pasted image 20260329165725.png]]回到iss53继续工作

工作完成，合并且删除iss53
```console
git checkout master
```

```console
git branch -d iss53
```

![[Pasted image 20260329165943.png]]此时C6有两个父提交，git内部自行作的简单合并

- 删除远程分支
```
git push origin --delete 分支名
```
## 遇到冲突时的分支合并

- 例如hotfix和iss53处理到了同一个文件，会发生冲突
- 此时 Git 做了合并，但是没有自动地创建一个新的合并提交。 Git 会暂停下来，等待你去解决合并产生的冲突。 你可以在合并冲突后的任意时刻使用 `git status` 命令来查看那些因包含合并冲突而处于未合并（unmerged）状态的文件
```console
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:      index.html

no changes added to commit (use "git add" and/or "git commit -a")
```
- 任何因包含合并冲突而有待解决的文件，都会以未合并状态标识出来。 Git 会在有冲突的文件中加入标准的冲突解决标记，这样你可以打开这些包含冲突的文件然后手动解决冲突。 
- 冲突解决完成后对修改冲突的文件使用**git add** 标记为冲突已解决
使用**git commit** 来完成合并提交

## 查看提交历史

```
git log
```

## 撤销暂存操作

- 漏有文件进行add, 或者提交信息写错。这里直接给一个例子，运行后只会有一个提交
```console
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```
- add了错误的文件，使用**reset** 来进行取消暂存。直接举例
```console
git reset HEAD CONTRIBUTING.md
```
- 撤销对文件所做的修改，将其返回到上一次提交时的样子。
```console
git checkout -- CONTRIBUTING.md
```
## 添加远程仓库
- 运行git remote add [shortname]  [url]  以添加一个新的远程仓库，并添加一个方便的缩写

```console
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
pb	https://github.com/paulboone/ticgit (fetch)
pb	https://github.com/paulboone/ticgit (push)
```
- 从远程仓库中抓取与拉取
```console
git fetch <remote>
```
这个命令会访问远程仓库，从中拉取所有你还没有的数据。 执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看

如果你使用 `clone` 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为简写。 所以，`git fetch origin` 会抓取克隆（或上一次抓取）后新推送的所有工作。

它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。

- 推送到远程仓库
```console
git push origin main
```
将main分支添加到远程仓库origin中

- 查看某一个远程仓库的更多信息
```
git remote show <remote>
```
## 远程仓库的重命名与移除
- 重命名
```console
$ git remote rename pb paul
$ git remote
origin
paul
```
- 移除远程仓库
```console
$ git remote remove paul
$ git remote
origin
```
## 简单的协作开发

- 先fork
克隆别人仓库一套副本，归属权划到自己仓库，自己拥有修改提交的权力。但是没有修改main分支的权限。main分支永远与原作者保持同步。

推送分支到远程后，可以在github上手动选择**compare(对比)**  或者**Pull Request(合并分支)** 