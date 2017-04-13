---
title: 在 git 中取消追蹤檔案 | Ignore tracked files in git
date: 2017-04-13 11:36:25
categories: Dev
tags: [git, gitignore]
---
這篇簡單介紹要怎麼 ignore 你已經 tracked 的文件

## Method 1 : `git rm`

### Step 1: Run this comment
``` shell
$ git rm --cached filename         # for file
$ git rm -r --cached foldername    # for folder
```

### Step 2: edit .gitignore
把你要 ignore 的檔案寫進 `.gitignore`

### Step 3: commit
``` shell
$ git commit -am "modify .gitignore"
```

如果之後想要重新 track 該 file，只要重新 `git add` & 編輯 .gitignore 即可

## Method 2 : `git update-index`
**to untracked file**
``` shell
$ git update-index --assume-unchanged <file>
```

**to retracked file**
``` shell
$ git update-index --no-assume-unchanged <file>
```

這個方法一樣可以 *untracked file*，但是這個比較像是 **暫時性處置**
如果你有一些 file 只是暫時不想要被紀錄，可以用這個方式移除
如果你是想要永久性的 untracked，請使用 `git rm`


# Reference
- [git rm](https://git-scm.com/docs/git-rm)
- [git update-index](https://git-scm.com/docs/git-update-index)
- [Ignore files that have already been committed to a Git repository](http://stackoverflow.com/questions/1139762/ignore-files-that-have-already-been-committed-to-a-git-repository)
- [Difference between `git rm --cached` and `git update-index --assume-unchanged`?](http://stackoverflow.com/questions/33428702/difference-between-git-rm-cached-and-git-update-index-assume-unchanged)
