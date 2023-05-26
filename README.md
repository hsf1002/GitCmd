# Git-cmd
git common commands

### 信息配置
```
git config --list  
git config --global user.name sky
git config --global user.email   hsf1002@gmail.com  
git config --global core.editor vi  
git config --global merge.too vimdiff  
git config --global alias.st "status"
git config --global alias.ck "checkout"
git config --global credential.helper store   (防止github提交时重复输入用户名密码)

git config --local： 对本次仓库有效
git config --global：对当前用户（所有仓库）有效
git config --system：对所有用户有效

对于.git/config配置而言，git config --local优先级最高
```

### 四种协议

```
1. 本地协议
第一种情况：remote--->local
(1) 克隆一个远程版本库到本地
<哑协议，不推荐>git clone /Users/sky/work/practice/git_test/remote
<智能协议，推荐>git clone file:///Users/sky/work/practice/git_test/remote
(2) 增加一个本地版本库到远程
git remote add local_proj /Users/sky/work/practice/git_test/remote
(3) 本地修改后同步到远程
git push local_proj

第二种情况：local--->remote
(1) 远程新建一个git仓库
git init
(2) 增加一个本地版本库到远程
git remote add local_proj /Users/sky/work/practice/git_test/remote
(3) 本地修改后同步到远程
git push local_proj

为推送当前分支并建立与远程上游的跟踪，使用：    git push --set-upstream remote_file master

如果报错，需要在远程的.git/config中添加
[receive]
    denyCurrentBranch = ignore

2. 智能 HTTP 协议
最流行，既支持像 git:// 协议一样设置匿名服务，也可以像 SSH 协议一样提供传输时的授权和加密，相比 SSH 协议，可以使用用户名／密码授权，用户不必在使用 Git 之前先在本地生成 SSH 密钥对再把公钥上传到服务器

克隆一个远程版本库到本地
git clone https://github.com/hsf1002/GitCmd.git

哑 HTTP 协议
在 Git1.6.6 版本之前，十分简单且通常是只读模式

3. SSH 协议
所有传输数据都要经过授权和加密，与 HTTP/S 协议、Git协议及本地协议一样，很高效，在传输前也会尽量压缩数据

克隆一个远程版本库到本地
git clone ssh://user@server/project.git
git clone user@server:project.git
不指定用户，Git 会使用当前登录的用户名

4. Git 协议
监听在一个特定的端口（9418），类似 SSH 服务，但是访问无需任何授权，要么谁都可以克隆这个版本库，要么谁也不能
```

### 五种状态

```
untracked、unmodifed、modified、staged、committed
```

### 初始化并提交
```
git init  
git add .             未跟踪->已跟踪为修改  
git commit -m         提交更新   
git commit -am        跳过暂存区
```

### .gitignore  
```
*.[ao]        任何.a或者.o文件  
out           当前目录下以及所有子目录下的out目录和out文件  
out/          out目录下所有文件  
/out          out目录和out文件，子目录下的out不包括  

/tmp/models/*
!/tmp/models/empty   忽略 /tmp/models 文件夹下的所有文件，但不忽略 /tmp/models/empty 文件


git工程配置以后，再添加.gitignore将无法生效，需要
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```

### 查看差分
```
git diff           工作区 – 暂存区  
git diff HEAD      工作区 – 提交区
git diff –cached   暂存区 – 提交区
git diff master test --filename  master分支和test分支关于filename的差异
```

### 查看类型与内容

```
cat-file -t 325b5488e09c8d39c655f0d0abb497b307acf517
commit
git cat-file -p 325b5488e09c8d39c655f0d0abb497b307acf517
tree 52299e7bc9aeae5aa3a7338615bb09a7e73d2773      // 隶属那个tree
parent 3dc8cb1dc564d14b3525ae192d57411b90ac8168    // 上一个提交
author sky <hsf1002@gmail.com> 1576928487 +0800    // 原作者
committer sky <hsf1002@gmail.com> 1576928487 +0800 // 提交者

26033


git cat-file -t fdd196c2099dc5df5aa22d086b47693d4b7a14cd
tree
git cat-file -p fdd196c2099dc5df5aa22d086b47693d4b7a14cd
100644 blob 21955369dca020b1c3f3ccf61484e2459a0eb4d1	README.md
git cat-file -t 21955369dca020b1c3f3ccf61484e2459a0eb4d1
blob
git cat-file -p 21955369dca020b1c3f3ccf61484e2459a0eb4d1
### 第26章 监控子进程
```

### 删除多余

```
git rm $(git ls-files –deleted)  删除多个已删除的文件  
git rm  从工作目录（working tree） 删除  
git rm –f         从工作目录和暂存区删除  
git rm –cached    从暂存区删除  

find . -name .gitignore | xargs rm -rf  
find . -name .git | xargs rm -rf
```

### HEAD指针

```
HEAD总是指向一个commit，如果是最近一个commit，则指向该分支
否则是分离指针，表示不属于任何分支

查看当前HEAD指针：cat .git/HEAD
ref: refs/heads/master
```

### 修改提交

```
如果是本次提交：git commit --amend
如果不是本地提交：git rebase -i SHA（要修改的提交的父亲）
需要修改的提交pick改为r
:wq!退出

git会先分离HEAD指针，修改之后重新提交再进行rebase，所以SHA信息会改变
```

### 合并提交

```
git rebase -i SHA（最早要合并的提交的父亲）
最早要合并的提交放在第一行
其他要合并的提交修改pick为s
:wq!退出

git会先分离HEAD指针，修改之后重新提交再进行rebase，所以SHA信息会改变
```

### 复制节点

```
git cherry-pick SHA  -10        将另一个分支的SHA之前的10次提交复制到当前分支  
git cherry-pick --theirs file   将对方分支文件检出
git cherry-pick --ours file     将本地分支文件检出
提示 refusing to lose untracked file  engmode/..../TelephoyFragment.java：此工程把该文件过滤掉了
提示 is not possible because you have unmerged files：此工程把cherry-pick文件过滤掉了
```

### 备份栈

```
git stash        备份当前工作区内容  
git stash pop    读取当前工作区内容，并清空栈  
git stash list   查看当前栈内备份列表  
git stash apply stash@{2}    读取第二次保存内容，但不会清空栈 
```

### PATCH

```
git format-patch  -1           当前提交生成的patch  
git format-patch SHA1          从SHA1到当前提交生成的patch  
git format-patch SHA1..SHA2    SHA1到SHA2之间的patch（SHA2比SHA1新）  
git am patch    打patch  
 
git diff SHA1           SHA1基于上次提交生成的patch  
git diff SHA1 SHA2      SHA2基于SHA1生成的patch  
git apply –check patch  打patch前判断能否顺利执行  
git apply patch         打patch  
git patch –p1 < patch   先进入patch指示的目录，将patch拷贝到该目录，执行此命令(不支持二进制和so等文件)
```

### 标签

```
git tag           查看标签
git tag V1.0      创建轻量标签
git tag V1.0 SHA  给SHA创建轻量标签
git tag -a V1.0 -m "the first tag" SHA  给SHA创建附加标签
git tag -d V1.0   删除标签
```

### 移动文件

```
git mv README.md README
相当于
mv README.md README 
git rm README.md 
git add README
```

### 查看日志

```
git log master --oneline -10        单行查看master最近10次提交  
git log master --pretty=oneline -10 单行查看master最近10次提交  
git log master --pretty=oneline --graph -10         以图形方式单行查看master最近10次提交
git log masger --pretty=format: “%s” -10 > log.txt  最近10次提交格式显示后写入文件  
git log –p – filepath/file          查看file在历史哪些提交有过更新 
git reflog                          查看引用分支，所有的git操作都将记录，可配合reset回退  
```

### 查看提交文件列表

```
git show --stat --oneline HEAD  最近一次
git show --stat --oneline SHA   某一次

git show --name-only --oneline HEAD  不显示添加/删除统计信息
```

### 回退文件到某次提交

```
git log filepath        列出文件所有提交
git reset SHA filepath  讲文件回退到SHA
```

### 撤销与恢复

```
checkout：针对工作区
reset：针对暂存区或提交区

git checkout -- file    丢弃工作区的修改  
git reset HEAD file     回到已修改未暂存的状态  
 
git reset HEAD~1        撤销最近一次提交  
git reset HEAD^         撤销最近一次提交  
git reset SHA           回到SHA的状态  
git reset --soft        回退提交区到暂存区
git reset --mixed       回退暂存区到工作区
git reset --hard        回退整个提交
       
git revert HEAD         撤销上一次提交  
git revert HEAD^        撤销上上一次提交  
git revert SHA          撤销SHA的提交  
```

### revert与reset

```
reest:  HEAD向后移动
revert: HEAD继续向前
```

### merge与rebase

```
merge: 会将两个分支最近的节点合并生成一个新的节点，原先两个分支的节点都保留，非线性结构，如果想保存两个分支完整的历史记录，避免重写commit history的风险，应该merge  

rebase: 会在两分支的结合节点依次cherry-pick分支的节点，然后依次更新master上后面分支节点，保证线性，如果想要一个干净的没有merge节点的线性历史树，应该rebase 
```

### 调试

```
git blame -C -L 38,42 RkLunch.sh   查看文件第38-42行的所有提交记录，-C表示即使文件重命名也可以查看
```

### 分支

```
git branch  newbranch    新建分支  
git checkout newbranch   切换分支（新建并切换分支：git checkout –b newbranch）  
git branch –d newbranch  删除分支（D表示强制删除）  
git branch –v            查看远程git库的路径
git branch -r            查看远程分支
```

### 远程分支

```
git remote show cmcc      查看信息
git remote rename cmcc mp 重命名
git remote rm cmcc        删除

git remote add shiyang_L713 ssh://user@192.168.30.160/home/workspace/Work/Mehfil_GIT/idh.code/.git  localpath(localpath可选，为shiyang_L713的对应的远程的本地分支)
git fetch cmcc    将远程分支代码同步下来到cmcc（git pull则是先同步再merge）
git push cmcc master:base_cmcc  将本地master分支push到远程base_cmcc分支  
git push cmcc :base_cmcc        先删除远程cmcc的base_cmcc分支  
git push -u origin master -f    往github push时候出错，如果要直接覆盖github上代码
git clone ssh://shiyang@192.168.0.25/home/29/shiyang/work/qiku    克隆某个工程到本地  
```

### 与远程分支同步

```
同步远程S809_Aurora分支到本地
git rebase origin/S809_Aurora  // 可能要处理冲突，分支保持线性
git pull origin/S809_Aurora    // 相对容易，但是分支看起来会乱

1. 本地push到远程S809_Aurora_ctcc分支
git pull --rebase origin S809_Aurora_ctcc
git rebase --skip
git push origin S809_Aurora_ctcc

2. 强制push到服务器
git push origin S809_Aurora_ctcc -f

3. 强制更新到本地
git fetch origin
git reset --hard origin/S809_Aurora_ctcc
```

### 与Github同步

```
1. 如果github是空，且本地也为空
先在github创建仓库，在本地目录执行：git clone https://github.com/hsf1002/GitCmd.git

2. 如果github是空，且本地不为空
a: 先在github创建仓库（名称为https://github.com/hsf1002/GitCmd.git）
b: 本地创建远程分支：git remote add github https://github.com/hsf1002/GitCmd.git
c: 将远程github的master分支同步下来：git fetch github master
d: 将两个不相干的分支（远端和本地）进行合并：git merge --allow-unrelated-histories github/master
e: 将本地所有分支push到远端：git push github --all
```

### 在github搜索

```
搜索仓库
jquery in:name             匹配其名称中含有 jquery 的仓库
jquery in:name,description 匹配其名称或说明中含有 jquery 的仓库
jquery in:readme           匹配其自述文件中提及 jquery 的仓库
repo:hsf1002/hello-world   匹配用户hsf1002下的特定仓库名称

user:hsf1002 forks:>1000 followers:>100  匹配fork超过1000，跟随者超过100个，来自hsf1002的仓库
jquery in:name stars:>1000 size:>100     匹配星星超过1000，大小超过100K，名称包含jquery的仓库
jquery created:>2019-11-11               匹配具有jquery字样、日期在 2019-11-11之后创建的仓库
jquery pushed:>2019-11-11 fork:only      匹配具有jquery字样，日期在 2019-11-11之后收到推送并且作为复刻的仓库
serialport language:C++ topics:>1 topic:QT5  匹配具有serialport字样，开发语言是C++，主题超过1个并且主题包含QT5的仓库

搜索代码：
user:hsf1002 extension:cpp availablePorts in:file              在用户hsf1002的cpp文件中搜索包含availablePorts的代码片段
repo:hsf1002/qt-serial-port extension:cpp QSerialPort in:file  在用户hsf1002的仓库qt-serial-port的cpp文件中搜索包含QSerialPort的代码片段
```

### 从repo远程下载
```
mkdir -p ~/.bin
PATH="${HOME}/.bin:${PATH}"
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo
chmod a+rx ~/.bin/repo

export REPO_URL='https://mirrors.tuna.tsinghua.edu.cn/git/git-repo'
cat ~/.ssh/id_rsa.pub 
repo init -u ssh://Zhangming@192.168.0.74:29418/manifests.git -b CNCE/chiron/master -m cnce_yandex_default.xml
repo sync
repo start master --all
```
