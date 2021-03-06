## 暂存区和HEAD概念

* 暂存区，称为stage或index，是一个介于工作区和版本库的中间状态，当执行提交时，实际上是将暂存区的内容提交到版本库中。
* HEAD实际是指向当前分支的一个游标，代表版本库中最近的一次提交
* 符号^可以用于指代父提交
  1. HEAD^代表版本库中的上一次提交，即最近一次提交的父提交
  2. HEAD^^则代表HEAD^的父提交
  3. 对于一个提交有多个父提交，可以在符号^后面用数字表示是第几个父提交


## git config
- 配置生效优先级：`--local` > `--global` > `--system`
  1. `/etc/gitconfig` 对所有用户都适用的配置，使用`git cofing --system`
  2. `~/.gitconfig` 用户目录下的配置，使用`git config --global`
  3. `.git/config` 工作目录下的配置，使用`git config --local`
- `git config [options]`
- `git config -e [<--system> | <--global> | <--local>]` 打开文本编辑器进行编辑，省略配置文件位置时，则选择工作目录下的配置
- `git config -l | --list` 查看配置信息
- `git config --global user.name "John Doe"`
- `git config --global user.email johndoe@example.com`
- `git config --global core.editor vim` 设置文本编辑器
- `git config --global color.ui true`
- `git config --global alias.st status` 设置别名
- `git config receive.denyNonFastForwards true` 禁止非快进试推送
- `git config --global merge.tool kdiff3`
- `git config --global core.autocrlf false/true/input`
  1. 在`Windows`系统上，把它设置成`true`，这样当签出代码时，`LF`会被转换成`CRLF`
  2. `Linux`或`Mac`系统上，因此你不想`Git` 在签出文件时进行自动的转换；当一个以`CRLF`为行结束符的文件不小心被引入时你肯定想进行修正，设置成`input`来告诉`Git`在提交时把`CRLF`转换成`LF`，签出时不转换
  3.  如果仅运行在`Windows`上的项目，可以设置`false`取消此功能，把回车符记录在库中
- `git config --global core.whitespace trailing-space,space-before-tab,indent-with-non-tab`
  1.  默认被打开的2个选项是`trailing-space`和`space-before-tab`，`trailing-space`会查找每行结尾的空格，`space-before-tab`会查找每行开头的制表符前的空格。
  2.  默认被关闭的2个选项是`indent-with-non-tab`和`cr-at-eol`，`indent-with-non-tab`会查找8个以上空格（非制表符）开头的行，`cr-at-eol`让`Git`知道行尾回车符是合法的。
- `git config --global diff.word.textconv strings` 在比较前把`Word`文件转换成文本文件
- `git config user.name 查看某个环境变量的设定
- `git config --global http.proxy http://130.147.222.32:808` 设置`http`代理，适用于`http_proxy`, `https_proxy`
  - 或者直接在命令行中设置
   ```
   export HTTPS_PROXY=http://130.147.222.32:808
   ```
  - 或者在~/.bashrc中设置
   ```
   export http_proxy=http://130.147.222.32:808
   export https_proxy=http://130.147.222.32:808
   export ftp_proxy=http://130.147.222.32:808
   export no_proxy='.example.com'
   ```

## git clone

1 git clone <repository> <directory>
2 git clone --bare <repository> <directory.git>
3 git clone --mirror <repository> <directory.git>
 1) 用法1将<repository>指向的版本库创建一个克隆到<directory>目录。目录<directory相当于克隆版本库的工作区，文件都会检出，版本库位于工作区下.git目录中
 2) 用法2和用法3创建的克隆版本库都不包含工作区，直接就是版本库的内容，这样的版本库称为裸版本库。一般约定俗成裸版本库的目录名以.git为后缀，所以上面实例中将克隆出来的裸版本库目录名写作<directory.git>
 3) 用法3和用法2的区别就是克隆出来的裸版本对上游版本进行了注册，这样可以在裸版本库中使用git fetch命令和上游版本库进行持续同步

git clone file:///opt/git/project.git
git clone ssh://user@server:project.git
git clone http://example.com/gitproject.git
git clone git@github.com:User/progit.git

SSH key
ssh-keygen 生成公钥（~/.ssh/id_dsa.pub）和私钥（~/.ssh/id_dsa）

## git remote

git remote add [shortname] [url] 添加远程仓库
git remote set-url <shortname> <newurl> 设置远程仓库新的地址信息
git remote -v 显示当前项目所有的远程仓库信息
git remote show 显示所有的远程仓库的简单名称
git remote show [shortname] 查看某个远程仓库的详细信息
git remote rename <old> <new> 修改某个远程仓库的简短名称
git remote rm | remove <shortname> 移除某个远程仓库配置信息，只是本地移除
git fetch [shortname] [branch-name] 从远程仓库抓取数据，只是将远端的数据拉到本地仓库，并不自动合并到当前工作分支
git pull [shortname] [branch-name] 自动抓取数据下来，然后将远端分支自动合并到本地仓库中当前分支
git pull [shortname] --tags 自动抓取远程仓库上所有的分支
git push [shortname] [branch-name] 将本地仓库中的数据推送到远程仓库某个分支上
git push [shortname] [tagname] 默认情况下，push并不会把标签传送到远端服务器上，只有通过显式命令才能推送标签到远端仓库。
git push [shortname] --tags 推送所有的分支到远端服务器上（注意是否有权限）
git push [远程名] [本地分支]:[远程分支] 如果省略[本地分支]，那就等于是在说：在这里提取空白然后把它变成[远程分支]，即删除掉[远程分支]了，[本地分支]不会删除

## git add

巧用git add
假设已经开发好了某个功能，这时候用git add添加，但不commit。
场景一：这时候我再去添加些测试代码测试，比如System.out.println这种代码，测试下来发现我刚才add的代码都是OK的，这时候我用git checkout直接就能删掉刚才添加的测试代码。
场景二：我发现刚才add的代码不够完美，修改完成之后，再次用git add添加，就会把上一次add的代码合并了，然后做为一次commit。

## git status

git status -s 精简状态输出
 1) 位于第一列的字符M（绿色）的含义：版本库中的文件与暂存区中的文件相比有改动
 2) 位于第二列的字符M（红色）的含义：工作区当前的文件与暂存区的文件相比有改动
git status -sb 同时显示所在的分支
git cherry 查看哪些提交领先，未被推送到上游跟踪分支中

## git diff

比较差异
git diff 比较工作区和暂存区
git diff --word-diff 逐词比较，而非默认的逐行比较
git diff --cached 比较暂存区和HEAD
git diff --staged 等同于git diff --cached，Git 1.6.1之后支持。
git diff HEAD 比较工作区和HEAD
git diff <$id1> <$id2> 用SHA1 ID值（前7位即可）比较两次提交之间的差异。注意顺序不一样，查看到的结果也会不一样。
git diff <branch1> <branch2> 在两个分支之间比较，只比较已经提交的内容，同样可以比较远程分支（ git diff pro/master pro/b1，注：先执行git fetch）
git diff <branch2> [current-branch] 用当前分支比较另外一个分支，同样可以比较远程分支（ git diff pro/b1，注：先执行git fetch）
git diff <任意有效可以进行比较的> 比较工作区和目标
git diff --stat 比较之后，显示简要的增改行数统计
git diff --binary 比较二进制文件差异
git checkout [HEAD] 汇总显示工作区、暂存区与HEAD的差异

## git stash

暂存
git stash 保存当前的工作进度，会分别对暂存区和工作区的状态进行保存
git stash list 显示所有stash
git show <stash@{n}> 查看某次暂存修改的内容
git stash pop [--index] [<stash@{n}>]
 1) 如果不使用任何参数，会恢复最新保存的工作进度，并将恢复的工作进度从存储的工作进度列表中清除
 2) 如果使用stash参数，则从该stash中恢复，恢复完毕也将从进度列表中删除该stash
 3) --index除了恢复工作区的文件外，还尝试恢复暂存区。
git stash apply [<stash@{n}>] [--index] 恢复暂存的内容，不会删除暂存。
  --index用于恢复被add过的变更，这样能恢复到暂存区状态。没有指定第几个暂存时，则使用最近的一次暂存。
git stash [save [--patch] [-k|--[no-]keep-index] [-q|--quiet] [<message>]
 1) 这条命令是git stash命令的完成版。如果需要在保存暂存的时候指定说明，使用格式：git stash save "Message"
 2) 使用参数--patch会显示工作区和HEAD的差异，通过对比差异文件的编辑决定在进度中最终要保存的工作的内容，通过编辑差异文件可以在进度中排除无关内容
 3) 使用-k或--keep-index参数，在保存进度后不将暂存区重置（默认会将暂存区和工作区强制重置）
git stash drop [<stash@{n}>] 删除暂存，没有指定第几个暂存时，默认会删除最近的一次暂存
git stash clear 删除所有的暂存
git stash branch <branch> [<stash@{n}>] 这会创建一个新的分支，并切换到新分支，检出你储藏工作时的所处的提交，重新应用你的工作，如果commit成功，将会删除暂存
git reflog show refs/stash


## 撤销操作

git commit --amend 修改最后一次提交，退出编辑器之后会直接commit。如果要取消，则在编辑器里清空掉提交信息再退出即可。使用场景如：修改提交信息、补正刚才修改的内容（需要先add）、少添加文件了（需要先add），谨慎操作！！
git checkout [--] <file> 用暂存区中的文件覆盖工作区中的文件，相当于取消自上次执行add以来的本地修改
git checkout <branch> [--] <file> 维持HEAD的指向不变，用branch所指向的提交的文件替换缓存区和工作区中相应的文件
git checkout [--] <. | file> 会用暂存区全部的文件或者指定的文件替换工作区的文件，会清除工作区中未添加到暂存区的改动
git checkout HEAD <. | file> 会用HEAD指向的分支中的全部或部分文件替换暂存区和工作区中的文件，不但会清除工作区未提交的改动，也会清除暂存区中未提交的改动
git reflog 查看引用日志，是HEAD头指针的变迁记录，而非master分支
git show HEAD@{n} 使用引用日志的输出中的 @n 引用
git reset [--soft | --hard | --hard ] [<commit>]
 1) 使用参数--hard，如：git reset --hard <commit>
    会替换引用的指向，引用指向新的提交ID
    替换暂存区，替换后，暂存区的内容和引用指向的目录树一致
    替换工作区，替换后，工作区的内容和暂存区一致，也和HEAD所指向的目录树内容相同
 2) 使用参数--sort，如：git reset --sort <commit>
    只更改引用的指向，不改变暂存区和工作区
 3) 使用参数--mixed或不使用参数（默认为--mixed），如：git reset <commit>
    更改引用的指向及重置暂存区，但不改变工作区
git reset [HEAD] 仅用HEAD指向的目录树重置暂存区，工作区不会收到影响，相当于将之前add到暂存区的内容撤出暂存区。引用也未改变，因为引用重置到HEAD相当于没有重置
git reset HEAD <file> 把文件从暂存区状态恢复到工作区状态，文件内容不会改变
git reset [--] <file> 效果同命令：git reset HEAD <file>
git reset --soft HEAD^
 1) 工作区和暂存区不改变，但是引用向前回退一次。当对最新提交的提交说明或提交的更改不满意时，撤销最新的提交以便重新提交
 2) 修补提交命令git commit --amend，用于对最新的提交进行重新提交以修补错误的提交说明或错误的提交文件。修补提交命令实际上相当于执行了下面两条命令。（注：文件.git/COMMIT_EDITMSG保存了上次的提交日志）
    git reset --soft HEAD^
    git commit -e -F .git/COMMIT_EDITMSG
git reset --hard HEAD^ 彻底撤销最近的提交。引用回退到前一次，而且工作区和暂存区都会回退到上一次提交的状态。自上一次以来的提交全部丢失


## git rebase

1 git rebase --onto <newbase> <since> <till>
2 git rebase --onto <newbase> <since>
3 git rebase <since> <till>
4 git rebase <since>
针对1/2/3/4变基操作的过程
 1) 首先会执行git checkout切换到<till>。由于会切换到<till>，因此如果<till>指向的不是一个分支，则变基操作是在detached HEAD（分离头指针）状态进行的，当变基结束后，还要对分支执行重置以实现变基结果在分支中生效
 2) <since>..<till>所表示的提交范围写到一个临时文件中。<since>..<till>是指包括<till>的所有历史提交排除<since>及<since>的历史提交后形成的版本范围
 3) 将当前分支强制重置（git reset --hard）到<newbase>，相当于执行：git reset --hard <newbase>
 4) 从保存在临时文件中的提交列表中，将提交逐一按顺序重新提交到重置之后的分支上
 5) 如果遇到提交已经在分支中包含，则跳过该提交
 6) 如果在提交过程中遇到冲突，则变基过程暂停。用户解决冲突后，执行git rebase --continue继续变基操作。或者执行git rebase --skip跳过此提交。或者执行git rebase --abort就此终止变基操作切换到变基前的分支上
git rebase --continue 在变基遇到冲突而暂停的情况下，先完成冲突解决（添加到暂存区，不提交），然后在恢复变基操作的时候使用该命令
git rebase –abort 在变基遇到冲突而暂停的情况下，跳过当前提交的时候使用，谨慎用
git rebase –skip 在变基遇到冲突而暂停的情况下，终止变基操作，回到之前的分支时候使用
git rebase -i <commit> 执行交互式变基操作，会将<since>..<till>的提交悉数罗列在一个文件中，然后自动打开一个编辑器来编辑这个文件。可以通过修改文件的内容设定变基操作，实现删除提交、将多个提交压缩为一个提交、更改提交的顺序，以及更改历史提交的提交说明等。
 1) 开头的几行由上到下依次对应历史的提交
 2) 默认的动作都是pick，即应用此提交
 3) 动作reword，或简写r。在变基时会应用此提交，但是在提交的时候允许用户修改提交说明
 4) 动作edit，或简写e。在变基时会应用此提交，但是在应用后暂停编辑，提示用户使用git commit --amend执行提交，以便对提交进行修补。当用户执行git commit --amend完成提交后，还需要执行git rebase --continue继续变基操作。用户在变基暂停状态下可以执行多次提交，从而实现把一个提交分解为多个提交
 5) 动作squash，简写s。该提交会与前面的提交压缩为一个
 6) 动作fixup，简写f。类似动作squash，但是提交的提交说明被丢弃
 7) 可以通过修改变基任务文件中各个提交的先后顺序，进而改变最终变基后提交的先后顺序
 8) 可以通过改变变基任务文件，删除包含相应提交的行，这样该提交就不会被应用，进而在变基后的提交中被删除
git rebase origin/master 在执行git fetch命令后使用，如果工作区有未提交的更改，变基动作不会执行


## git branch

git branch 显示所有分支名称
git branch -v 显示所有分支最后提交信息
git branch <new_branch> 创建新分支，但不会自动切换到这个分支上
git branch <new_branch> <commit> 基于提交创建新分支
git checkout tag_name 基于标签创建新分支，HEAD会进入到游离指针状态
git checkout -b <new_branch> tag_name 基于标签创建新分支，并自动切换到这个分支上
git checkout -b <new_branch> 创建新分支，并自动切换到这个分支上
git checkout -b [new_branch] [remote-shortname]/[branch] 创建新分支来跟踪远程分支，并自动切换到这个分支上
git branch -m <old_branch> <new_branch> 重命名分支，如果存在<new_branch>，拒绝执行重命名
git branch -M <old_branch> <new_branch> 重命名分支，如果存在<new_branch>，强制执行重命名
git branch -d <branch> 删除某个分支
git branch -D <branch> 强制删除某个分支，未被合并的分支被删除的时候需要强制
git branch --merged 查看已经被合并到当前分支的分支
git branch --no-merged 查看尚未被合并到当前分支的分支

合并：有未提交修改情况下，不要执行merge！遵守这条警告，防患于未然
git merge [options] <commit>...
 1) 合并操作的大多数情况，只须提供一个<commit>（提交ID或对应的引用：分支、里程碑等）作为参数。合并操作将<commit>对应的目录树和当前工作分支的目录树的内容进行合并，合并后的提交以当前分支的提交作为第一个父提交，以<commit>为第二个父提交。合并操作还支持将多个<commit>代表的分支和当前分支进行合并，过程类似
 2) 默认合并后的结果会自动提交，但是如果提供--no-commit选项，则合并后的结果会放入暂存区，用户可以对合并结果进行检查、更改，然后手动提交
git merge <branch> 将branch分支合并到当前分支
git merge origin/master --no-ff 不要Fast-Foward合并，会自动生成merge提交信息
git merge origin/master --no-ff -m"<Message>" 不要Fast-Foward合并，手动生成merge提交信息
git merge --abort 冲突时执行中止merge操作，merge manual中说，这条命令会尽力恢复到Merge之前的状态（可能失败！）。


## git tag

git tag 列出现有的标签
git tag -l 'v1.4.2.*' 标签匹配查询
git tag -d v1.0 删除某个标签
git show v1.0 查看相应标签的版本信息，并连同显示打标签时的提交对象
1）轻量级标签就像是个不会变化的分支，实际上它就是个指向特定提交对象的引用。如果只是临时性加注标签，或者不需要旁注额外信息，用轻量级标签也没问题。
  git tag v1.2 要创建这样的标签，一个-a，-s 或-m选项都不用，直接给出标签名字即可
2）含附注标签，实际上是存储在仓库中的一个独立对象，它有自身的校验和信息，包含着标签的名字，电子邮件地址和日期，以及标签说明，标签本身也允许使用GNU Privacy Guard (GPG) 来签署或验证。一般我们都建议使用含附注型的标签，以便保留相关信息。
  git tag -a v1.0 -m 'my version 1.0' -m选项则指定了对应的标签说明，如果在此选项后没有给出具体的说明内容，Git会启动文本编辑器供你输入。
  git tag -s v1.1 -m 'my signed 1.1 tag' 如果有自己的私钥，还可以用GPG来签署标签，只需要把之前的-a改为-s
  git tag -v [tag-name] 验证已经签署的标签。此命令会调用GPG 来验证签名，所以你需要有签署者的公钥，存放在keyring 中，才能验证


文件追溯
git blame <file> 会逐行显示文件，在每一行的行首显示此行最早是在什么版本引入的，由谁引入的
git blame -L <n,m> <file> 查看某几行

git ls-files 显示暂存区和版本库中的文件
git ls-files --with-tree=HEAD^ 查看历史版本的文件列表
git cat-file -p HEAD^:<file> 查看历史版本中文件是否存在


删除
git rm --cached <file> 从暂存区删除文件，工作区不会删除
git clean [options] 移除掉未被跟踪的文件和目录
  -nd 显示将要被删除的目录和文件
  -fd 真正开始强制删除多余的目录和文件

重命名
git mv <old_file_name> <new_file_name> 重命名文件，并且会添加到暂存区


查看提交历史
git log 显示提交历史信息
git log <branch> 显示分支提交历史信息
git log -(n) 仅显示最近的n条提交
git log <file> 显示该文件提交信息  
git log -p 显示每次提交的内容差异  
git log -p -2 仅显示最近的两次更新  
git log --stat 显示简要的增改行数统计
git log --date-order --date=iso --graph --full-history --all --pretty=format:'%x08%x09%C(red)%h %C(cyan)%ad%x08%x08%x08%x08%x08%x08%x08%x08%x08%x08%x08%x08%x08%x08%x08 %C(bold blue)%aN%C(reset)%C(bold yellow)%d %C(reset)%s'
 -p 按补丁格式显示每个更新之间的差异。
 --stat 显示每次更新的文件修改统计信息。
 --shortstat 只显示--stat 中最后的行数修改添加移除统计。
 --name-only 仅在提交信息后显示已修改的文件清单。
 --name-status 显示新增、修改、删除的文件清单。
 --abbrev-commit 仅显示SHA-1 的前几个字符，而非所有的40 个字符。
 --relative-date 使用较短的相对时间显示（比如，“2 weeks ago”）。
 --graph 显示ASCII 图形表示的分支合并历史。
 --pretty 使用其他格式显示历史提交信息。可用的选项包括oneline，short，full，fuller 和format（后跟指
定格式）。
 format选项说明
  %H 提交对象（commit）的完整哈希字串
  %h 提交对象的简短哈希字串
  %T 树对象（tree）的完整哈希字串
  %t 树对象的简短哈希字串
  %P 父对象（parent）的完整哈希字串
  %p 父对象的简短哈希字串
  %an 作者（author）的名字
  %ae 作者的电子邮件地址
  %ad 作者修订日期（可以用-date= 选项定制格式）
  %ar 作者修订日期，按多久以前的方式显示
  %cn 提交者(committer)的名字
  %ce 提交者的电子邮件地址
  %cd 提交日期
  %cr 提交日期，按多久以前的方式显示
  %s 提交说明
git log --pretty="%h:%s" --author=gitster --since="2008-10-01" --before="2008-11-01" --no-merges
--since, --after 仅显示指定时间之后的提交。
--until, --before 仅显示指定时间之前的提交。
--author 仅显示指定作者相关的提交。
--committer 仅显示指定提交者相关的提交。
git log --oneline --decorate 显示tag信息


忽略文件
1) 创建文件.gitignore添加规则
2) 编辑.git/info/exclude，独享
3) 对于已经被git管理的文件，使用以下方式忽略，但在git rebase操作之后，这些被忽略的文件内容如果被修改过了，会被强制恢复到git rebase这个时候的版本内容
  git update-index --assume-unchanged /path/to/file     #忽略跟踪
  git update-index --no-assume-unchanged /path/to/file  #恢复跟踪

git grep "工作区文件内容搜索"
gitk --all 显示所有的分支
gitk --since="[YYYY-MM-dd]|[2 days ago][2 weeks ago]..." 显示某天以来的所有提交
git gui

windows平台下的msysGit中shell环境配置
01) 编辑配置文件/etc/inputrc，修改或者添加以下配置，重启后就能在shell界面输入中文
# disable/enable 8bit input
set meta-flag on
set input-meta on
set output-meta on
set convert-meta off
02) ls命令显示中文，将alias命令添加到配置文件/etc/profile中
alias ls='ls --show-control-chars --color=auto'
alias gitk='gitk --all' #设置别名
3) 解决gitk和Git Gui乱码，在 "/etc/gitconfig" 中添加或修改以下配置
[gui]
    encoding = utf-8
4) 在/etc/git-completion.bash末尾位置添加“cd /d”，可以让msysGit打开定位到D盘


实战1：两个提交压缩为一个
1) 使用--soft参数调用充值命令，回到最近两次提交之前
git reset --soft HEAD^^
2) 查看版本状态和最新日志，如果需要修改就进行修改
3) 执行提交操作，即完成最新两个提交压缩为一个
git commit -m"Message"
