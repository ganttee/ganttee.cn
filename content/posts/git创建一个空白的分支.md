---
title: "Git创建一个空白的分支"
# description:
date: 2021-07-13T10:20:53+08:00
draft: true
tags:
- git
categories:
- git
---


### 1.创建一个空白的分支的需求
在Git中创建分支，是必须有一个父节点的，也就是说必须在已有的分支上来创建新的分支，如果工程已经进行了一段时间，这个时候是无法创建空分支的。但是有时候就是需要创建一个空白的分支。

### 2.解决方法：
#### 2.1 使用 git checkout的--orphan参数:
```
git checkout --orphan emptybranch
```
该命令会生成一个叫emptybranch的分支，该分支会包含父分支的所有文件。但新的分支不会指向任何以前的提交，就是它没有历史，如果你提交当前内容，那么这次提交就是这个分支的首次提交。

#### 2.2 删除所有文件：
想要空分支，所以需要把当前内容全部删除，用git命令
```
git rm -rf . //注意：最后的‘.’不能少。
```
#### 2.3 提交分支：
如果没有任何文件提交的话，分支是看不到的，所以我们需要创建一个新文件，然后提交则新创建的branch就会显示出来。
```
echo '# new branch' >> README.md

git add README.md

git commit -m 'new branch'
```
#### 2.4 最后push到远程仓库，则新的空分支就创建成功了。
```
git push origin emptybranch
```


参考：https://www.jianshu.com/p/a18487d987ac

