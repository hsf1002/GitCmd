# Git-cmd
git common commands

### 信息配置
```
git config --list  
git config –global user.name “sky”  
git config –global user.email   feng_he@xunrui.com.cn  
git config –global core.editor vi  
git config –global merge.too vimdiff  
```

### 删除多余
```
find . -name .gitignore | xargs rm -rf  
find . -name .git | xargs rm -rf
```

### 五种状态
```
untracked、unmodifed、modified、staged、committed
```

```
git init  
git add .     untracked->unmodifed  
git commit   -m:  提交更新 –a:  跳过暂存区  
git commit –amend     重新提交  
```

### .gitignore  
```
*.[ao]          任何.a或者.o文件  
!lib.a          除了lib.a之外  
out             out目录和out文件  
out/            out目录，非out文件  
/out  out目录和out文件，子目录下的out不包括                       
```

### 查看差分
```
git diff               working file – index file  
git diff HEAD             working file – commit file  
git diff –cached         index file – commit file
```

### 删除git多余
```
git rm $(git ls-files –deleted)             删除多个已删除的文件  
git rm  从工作目录（working tree）删除  
git rm –f  从工作目录和暂存区删除  
git rm –cached   从暂存区删除  
```

### 查看历史记录
```
git log master  –pretty=oneline -10       单行查看master最近10次提交  
git log masger –pretty = format: “%s” -10 > log.txt         最近10次提交格式显示后写入文件  
git log –p – filepath/file              查看file在历史哪些提交有过更新 
git reflog  查看引用分支，所有的git操作都将记录，可配合reset回退  
```

### 撤销恢复
```
git checkout -- file                       回退文件中工作区的修改  
git reset HEAD file                      回到已修改未暂存的状态  
 
git reset HEAD~1                       撤销最近一次提交  
git reset HEAD^                          撤销最近一次提交  
git reset SHA                               回到SHA的状态  
git reset –soft                              回退commit  
git reset –mixed                          回退commit、index  
git reset –hard                             回退commit、index、working  
       
git revert HEAD                         撤销上一次提交  
git revert HEAD^                       撤销上上一次提交  
git revert SHA                            撤销SHA的提交  
```

### 分支
```
git branch  newbranch     新建分支  
git checkout newbranch           切换分支（新建并切换分支：git checkout –b newbranch）  
git branch –d newbranch       删除分支（D表示强制删除）  
git branch –v                              查看各个分支的最近一次提交  
git checkout –theirs                   file    cherry-pick中冲突时使用拷贝源的文件  
```

### 复制节点
```
git cherry-pick SHA  -10          将另一个分支的SHA之前的10次提交复制到当前分支  
git cherry-pick --theirs file     将对方分支文件检出
git cherry-pick --ours file       将本地分支文件检出
git cherry-pick 提示 refusing to lose untracked file  engmode/..../TelephoyFragment.java，此工程把该文件过滤掉了
git cherry-pick 提示is not possible because you have unmerged files  此工程把cherry-pick文件过滤掉了
```
 
### 备份栈
```
git stash       备份当前工作区内容  
git stash pop 读取当前工作区内容，并清空栈  
git stash list   查看当前栈内备份列表  
git stash apply stash@{2}         读取第二次保存内容  
```

### 生成PATCH
```
git format-patch  -1 当前提交生成的patch  
git format-patch SHA1    从SHA1到当前提交生成的patch  
git format-patch SHA1..SHA2           SHA1到SHA2之间的patch （SHA2比SHA1新）  
git am patch    打patch  
 
git diff SHA1   SHA1基于上次提交生成的patch  
git diff SHA1 SHA2 SHA2基于SHA1生成的patch  
git apply –check patch  打patch前判断能否顺利执行  
git apply patch    打patch  
git patch –p1 < patch              先进入patch指示的目录，将patch拷贝到该目录，执行此命令
```

### 远程分支
```
git remote –v            查看远程git库的路径  
git remote add        git-base-cmcc  git@192.168.0.68:sprd9830_3.2.0_cmcc  localpath
         本地localpath新建一个名字为git-base-cmcc的对应于远程的本地分支  
git fetch           git-base-cmcc           将远程分支代码pull下来到git-base-cmcc  
git push  base_cmcc       master:base_cmcc  将本地master分支push到远程base_cmcc分支  
git push  base_cmcc       :base_cmcc      先删除远程base_cmcc的base_cmcc分支  
git clone ssh://shiyang@192.168.0.25/home/29/shiyang/work/qiku            克隆某个工程到本地  
```
