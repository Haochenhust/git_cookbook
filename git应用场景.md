<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [多个开发者同时使用同一个远程分支进行开发](#%E5%A4%9A%E4%B8%AA%E5%BC%80%E5%8F%91%E8%80%85%E5%90%8C%E6%97%B6%E4%BD%BF%E7%94%A8%E5%90%8C%E4%B8%80%E4%B8%AA%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF%E8%BF%9B%E8%A1%8C%E5%BC%80%E5%8F%91)
- [从远程仓库拉取分支，并在本地创建与之对应的分支](#%E4%BB%8E%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E6%8B%89%E5%8F%96%E5%88%86%E6%94%AF%E5%B9%B6%E5%9C%A8%E6%9C%AC%E5%9C%B0%E5%88%9B%E5%BB%BA%E4%B8%8E%E4%B9%8B%E5%AF%B9%E5%BA%94%E7%9A%84%E5%88%86%E6%94%AF)
- [将本地分支push到远程仓库，并新建分支](#%E5%B0%86%E6%9C%AC%E5%9C%B0%E5%88%86%E6%94%AFpush%E5%88%B0%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E5%B9%B6%E6%96%B0%E5%BB%BA%E5%88%86%E6%94%AF)
- [删除本地分支 & 远程分支](#%E5%88%A0%E9%99%A4%E6%9C%AC%E5%9C%B0%E5%88%86%E6%94%AF--%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF)
- [rebase操作](#rebase%E6%93%8D%E4%BD%9C)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 多个开发者同时使用同一个远程分支进行开发
三步走：
1. 本地的master分支要和远程的origin/master分支保持一致，切记不要在本地master分支上做任何本地的修改，本地master分支只用来和远程origin/master分支做同步（pull）
2. 另起一个本地分支：$master: git checkout -b dev, 在本地分支dev上开发自己的feature
3. 开发完成之后，如果发现远程分支有别的同学已经merge了代码，此时将master分支拉到本地，然后checkout -b tmp，tmp临时分支只是用于做提交用，之后切换到我们的本地开发分支dev,在dev分支上使用rebase操作：$dev: git rebase tmp, 此时本地开发分支，dev就和远程master分支的最新commit节点有了共同的祖先，所以就不会在merge的时候出现refused情况。
# 从远程仓库拉取分支，并在本地创建与之对应的分支
```gitignore
git fetch origin branch // 如果本地仓库没有dev分支,拉取远程仓库dev分支到本地git仓库
git checkout -b local_branch origin/branch // 在本地创建分支dev并切换到该分支
git pull origin branch // 拉取远程分支，并合并到本地分支
```
# 将本地分支push到远程仓库，并新建分支
```gitignore
git push --set-upstream origin xxx //将本地创建的分支 xxx push到远程仓库，同时在远程仓库创建该分支
```
# 删除本地分支 & 远程分支
```gitignore
// 删除本地分支
git branch -d localBranchName

// 删除远程分支
git push origin --delete remoteBranchName
```
# rebase操作
```gitignore
master$ git pull origin master //更新本地master分支
master$ git checkout -b tmp //创建临时分支，目的是要时刻保证master分支的干净
tmp$ git rebase dev 自己的开发的分支 // 要先切换到tmp分支，在执行git rebase这一步需要解决冲突
tmp$ git rvm //向gerrit提交CR
```
>将ab_test分支checkout到本地，然后将自己的branch rebase到ab_test分支上继续进行开发，之后git push --force origin recall_all_category将本地分支推送到远端分支，注意：rebase之后必须要使用force push，否则会出现问题。(Gerrit)
