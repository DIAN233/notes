# Git - 分布式版本控制系统

## 1. 安装Git

==Git 将每个版本独立管理==

## 2. 初次使用

### a. 配置

```
git config --global user.name "用户名"
git config --global user.eamail "邮箱"
```

==用英语！==

### b. 命令行

使用命令行进行操作。

```
win + R
win + X + A		//以管理员身份运行
```

#### 基本原理

- **记录**——Git 将每个版本独立保存。

- #### 三棵树——三个空间（地址）

- **工作区域、暂存区域、Git仓库** —— `Working Directory 、Stage(Index)、Repository(HAED)`

  ```mermaid
  graph TD
  工作区域-WorkingDirectory 
  暂存区域-Stage
  HEAD --> Git仓库-Repository
  ```

  

- ==**head 指针指向最新（最后）提交的文件**==

### c. 三种状态——三种提交状态

#### 工作流程：

1. 在工作目录中添加、修改文件

2. 将需要进行版本管理的文件放入暂存区域

3. 将暂存区域的文件提交到 Git 仓库

   

#### Git 管理文件的三种状态：

1. 已修改（modified）

2. 已暂存（staged）

3. 已提交（commited）

   

**创建文件 —— 跟踪管理文件**

```
git init
```

**添加文件 —— 放入暂存区域 （此时文件才受到跟踪）**

```
git add 文件名
```

**提交文件 —— 提交到 Git 仓库** 

```
git commit -m "做了什么"
```



## 3. 查看工作状态和历史状态

### a. 查看状态

```
git status
```

将最后一次提交的内容恢复到文件区

```
git reset HEAD 

git reset HEAD "文件名"
```

覆盖本地文件的修改 —— 使用未修改前的覆盖修改的——==有危险的==

```
git checkout -- 文件名
```

### b. 查看历史提交

```
git log 
```

git 会为每一次提交提供一个唯一的 ID 号



## 4.回到过去

### a. 回滚快照 

此时 HEAD 指针指向 Stage 区域，在工作区的是最新的文件版本，而在仓库区的是上一版的文件

```
git reset HEAD~		#加几个波浪线就是回到第几个
```

此时查看状态

```
git status

Changes not staged for commit:        #Git 检测到有新版本未被暂存
```

### b. reset 命令的选项

#### reset 命令回滚快照三部曲 —— 总结

- 移动 HEAD 的指向（ --soft ）
- 将快照回滚到暂存区域 （ [ --mixed ], 默认）
- 将暂存区域还原到工作目录 （ --hard ）

  

##### 移动两棵树：

1. 移动 HEAD 的指向，将其指向上一个快照
2. 将 HEAD 移动后的快照回滚到暂存区域

```
git reset --mixed HEAD~
```

##### 移动一棵树：

1. 移动 HEAD 的指向，将其指向上一个快照

```
git reset --soft HEAD~
```

##### 移动三棵树：

1. 移动 HEAD 的指向，将其指向上一个快照
2. 将 HEAD 移动后的快照回滚到暂存区域
3. 将暂存区域的文件还原到工作目录

```
git reset --hard HEAD~
```

==危险！ 覆盖工作目录（本地文件）最新版本！痛失更新！==

**此时 HEAD 指针指向 WorkingDirectory 区域，在工作区的是上一版的文件版本，在仓库区的也是上一版的文件。**

### c. 回滚指定快照

指定特定的快照 ID 进行回滚 —— 只需要 ID 号前几个 号码

```
git reset 版本快照的 ID 号
```

### d. 回滚个别文件

```
git reset 版本快照 文件名/路径
```

回滚个别文件，会忽略 HEAD 指针移动，HEAD 指针不改变

### e. 前滚

```
git reset 版本快照的 ID 号
```



## 5. 版本对比

### a. **修改最后一次提交**

- 版本提交（ commit ）到仓库，突然想起漏掉两个文件没有添加（ add）
- 版本提交（ commit ）到仓库，版本说明写的不够全面

==**执行带 --amend 选项的 commit 提交命令， Git 就会 “更正” 最近的一次提交，而不是一次新的提交**==

```
git commit --amend
```

进入界面修改提交说明 —— 只在原来的快照版本上面修改

```
esc + shift zz		//快捷键操作

! + Q + enter 		//快捷键

git commit --amend -m "做了什么"
```



### b. 删除文件

**恢复本地删除文件**

```
git checkout -- 文件名
```

**删除文件**

只删除在工作目录和暂存区域的文件 —— 取消跟踪，在下次提交时不纳入版本的管理

```
git rm 文件名
```

移动 HEAD 指向

```
git reset --soft HEAD~
```

**强制删除本地和暂存区域**

```
git rm -f 文件名
```

**只删除暂存区域**

```
git rm --cached 文件名
```

**重命名文件**

```
git mv 旧文件名 新文件名
```



## 6. Git 分支

### a. 创建分支

添加一个指向分支的指针

```
git branch 分支名
```

log 显示提交的所有引用，此时 HEAD 指针指向 master（主体）

```
git log --decorate
```

### b. 切换分支 —— HEAD 指针指向新建分支

```
git checkout 分支名
```

只显示快照 ID 和快照说明

```
git log --decorate --oneline
```

切换到主体，此时 HEAD 指针指向 master

```
git checkout master
```

图形化显示所有分支

```
git log --decorate --oneline --graph --all
```



## 7. 合并和删除分支

### a. 将分支合并到主体

```
git merge 分支名
```

出现冲突（conflicts）—— 两个分支中存在名同内容不同的文件

修改冲突 —— 提交提示所指的同名文件版本

```
git add 文件名

git commit -m "fix conflicts"
```

新创建并切换到分支

```
git checkout -b 文件名
```

### b. 删除分支

```
git branch -d 文件名		// --delete 
```

删除后快照名存在而文件名丢失 —— git 创建指针来指向分支



## 8. 匿名分支和 checkout 命令

### a. 匿名分支

没有指定分支名，此时创建一个匿名分支，在此所作的提交都被丢弃

```
git checkout HEAD~		//一个例子
```

当一些提交有风险时，使用匿名分支比较方便

### b. checkout 命令

**两种功能：**

1. 从历史快照（或暂存区域）中拷贝文件到工作目录
2. 切换分支

#### 从历史快照（或暂存区域）中拷贝文件到工作目录

**从指定的提交中拷贝文件到暂存区域和工作目录**

```
git checkout HEAD~ 文件名

git reset 
```

约定两个横（--）后边跟文件名

#### 切换分支

1. 添加一个指向快照的指针
2. 修改 HEAD 指针指向
3. 改变暂存区域和工作目录的内容

```
git checkout 分支名
```

只想恢复指定的文件或路径，只需要指定具体文件，此时 Git 不会修改 HEAD 指针指向

### c. checkout 命令和 reset 命令

#### 恢复文件

**同：**

- 都可以用于恢复指定快照的指定文件
- 都不改变 HEAD 指针指向

**不同:**

- reset 命令只将指定文件恢复到暂存区域（--mixed）—— 指定文件时，--soft 和 --hard 不允许使用
- checkout 命令是同时覆盖文件恢复到暂存区和工作目录

**==在恢复文件方面， reset 比 checkout 要安全==**

#### 恢复快照

- reset —— ==回到过去==

- checkout —— 用于切换分支，事实上是通过移动 HEAD 指针和覆盖暂存区域和工作目录来实现。

**相对于 reset --hard 命令，checkout 命令更安全：**

- checkout 命令在切换分支前会先检查工作状态，如果不是 ”clean" , Git 不会允许操作
- reset --hard 直接覆盖所有数据

**更新 HEAD 指向：**

- reset 移动 HEAD 所在分支的指向 —— 有危险的，将整个分支回滚，可能丢失内容
- checkout 移动 HEAD 自身来指向另一个分支

## 9. Github

