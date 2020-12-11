---
title: Git 重命名文件和文件夹
toc: false
date: 2020-12-11 14:44:06
tags:
categories:
    - Git
---

之前在git管理的项目中重命名文件和文件夹一直使用`mv`指令，例如：

```zsh
mv -r folder1 folder2
git add .
git commit "..."
```
这样会存在一个很大的问题，那就是这整个文件夹中的git历史信息删除掉了。正确的做法应该是：

- 文件夹

    ```zsh
    git mv -f folder1 folder2
    git add -u folder2
    git commit -m "..."
    ```
- 文件
    ```zsh
    git mv file1 file2
    git commit -m "..."
    ```
