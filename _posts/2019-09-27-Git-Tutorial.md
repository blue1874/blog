---
layout: post
title: "Git Tutorial"
subtitle: "最简单的Git使用"
date: 2019-09-27 19:53:00
author: "Loniclue"
header-img: "http://ww1.sinaimg.cn/large/0071Svkply1gkk44nvzl8j31hc0u045a.jpg"
header-mask: 0.3
catalog: true
tags: 
  - Git
  - Tutorial
---


Git常用工作流程  
--------------
![git flow](http://ww1.sinaimg.cn/large/0071Svkply1gkk44nv9xoj30sc0fdwez.jpg)

需要用到的命令  
-------------  
- [`git clone`][]  复制仓库，通常是从远程仓库复制到本地仓库，通常使用`git clone [-o <name>] [-b <name>] <repository> [<directory>]`，其中`-o <name>`表示使用`name`代替默认源仓库名(origin)，`-b <name>`表示使用`name`代替仓库默认主分支名(master),`<repository>`源仓库地址，远程仓库地址通常为[git urls][]。  
  

- [`git add`][]  跟踪修改，将文件修改添加到暂存区。使用形式`git add [<pathspec>…​]`， 其中`[<pathspec>…​]`指文件名，支持正则表达式。参数个人没怎么用，通常都是直接添加所有修改`git add .`。  
  

- [`git checkout`][] 用 `git checkout -- <file>` 撤销工作区该文件的修改，用`git checkout <branch>`切换分支
  

- [`git rm`][] 删除文件，等效于在文件管理器中删除文件。  
  

- [`git status`][] 查看文件状态，通常直接使用`git status`查看当前目录所有文件状态。文件状态有`updated` `deleted` `added`等，详见官方文档[git-status-output][]  
  

- [`git commit`][]  提交修改，将暂存区的文件修改提交。常用形式`git commit -m <msg>`， `-m <msg>`表示提交信息。  
  

- [`git pull`][]   拉取远程库上超前的修改并合并。指定远程库的情况下省略远程库的地址。使用`git pull --rebase`进行操作。  
  

- [`git push`][]  推送本地库到远程库，通常使用`git push [-u --set-upstream] [<repository> [<refspec>…​]]`， `-u`表示设置跟踪远程上游分支，在第一次推送该分支时使用。`[<repository> [<refspec>…​]]`表示远程仓库和要推送的本地分支，缺省时为默认的`origin master`。  
  

- [`git branch`][]  分支相关操作。`git branch`查看本地分支，添加`-a`参数查看所有分支，`-r`查看远程分支。`git branch <branch>`创建分支，`-d`删除分支。  
  

- [`git reset`][] 回退版本。`git reset --hard HEAD`回退上次commit后的版本，`HEAD^`上上次版本，以此类推。`git reset --hard commit_id`，回退到该版本号的版本。使用`git log`查看版本号  
  

[`git clone`]: https://git-scm.com/docs/git-clone "see Git docs"
[`git add`]: https://git-scm.com/docs/git-add "see Git docs"
[`git checkout`]: https://git-scm.com/docs/git-checkout "see Git docs"
[`git rm`]: https://git-scm.com/docs/git-rm "see Git docs"
[`git status`]: https://git-scm.com/docs/git-status "see Git docs"
[`git commit`]: https://git-scm.com/docs/git-commit "see Git docs"
[`git pull`]: https://git-scm.com/docs/git-pull "see Git docs"
[`git push`]: https://git-scm.com/docs/git-push "see Git docs"
[`git branch`]: https://git-scm.com/docs/git-branch "see Git docs"
[`git reset`]: https://git-scm.com/docs/git-reset "see Git docs"
[git urls]: https://git-scm.com/docs/git-clone#_git_urls_a_id_urls_a "see Git docs"
[git-status-output]: https://git-scm.com/docs/git-status#_output "see Git docs"