---
title: Git之大文件上传
date: 2025-05-09 22:39:56
tags: [Git, GitHub, Git的使用, GitHub上传, Git大文件上传]
categories: [Handbook]
---


# 概述

之前在使用Git上传电子书时出现了上传失败的问题，仔细查看发现失败的原因是因为需要上传的文件大小太大，导致上传失败，如下图所示。
![image-1](../images/Git之大文件上传/image-1.png#pic_center)

# 大文件上传

这时候可以尝试用Git LFS(Git Large File Storage，大文件存储)来解决这个问题。Git LFS可以简单的理解为存储大文本、视频、数据集的Git。以下是[官网](https://git-lfs.com/)的定义：
```
Git Large File Storage (LFS) replaces large files such as audio samples, videos, datasets, and graphics with text pointers inside Git, while storing the file contents on a remote server like GitHub.com or GitHub Enterprise.
```

具体使用方法如下。

第一步，先要恢复到没有上传大文件的那次commit记录，否则可能出现问题。使用如下命令查看Git上传记录。
```bash
git log
```
执行命令后如下图所示，其中绿色框内的2次提交记录就是失败的提交记录，黄色框内的提交记录为我们需要回滚的版本。
![image-4](../images/Git之大文件上传/image-4.png#pic_center)

使用如下命令回滚到之前版本。
```bash
git reset commit_id
```
执行完命令再使用`git log`查看提交记录，如下图所示，已经回滚到之前的版本。
![image-5](../images/Git之大文件上传/image-5.png#pic_center)

第二步，执行如下命令：
```bash
git lfs install
```
如果提示`git lfs initialized`则表示执行成功，如下图所示。
![image-2](../images/Git之大文件上传/image-2.png#pic_center)

第三步，执行如下命令追踪需要上传的大文件：
```bash
git lfs track "大文件名"
```
执行后如下图所示。
![image-3](../images/Git之大文件上传/image-3.png#pic_center)

第四步，做完上面的事情后，接着使用常规的Git上传命令即可上传文件。
```bash
git add .
git commit -m "上传大文件"
git push origin master
```