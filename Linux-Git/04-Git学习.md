# 一、认识 Git 和 GitHub

Git 是个版本控制系统，说明白点就是进行代码的各种管理，比如你写错代码进行回滚、追寻 BUG 是哪个家伙造成的、合并别人代码等等，从而达到协同进行软件开发工作。

这里要提下：版本管理控制系统分为「集中式版本控制系统」和「分布式版本控制系统」，很多人可能用过 SVN，就是属于集中式版本控制系统，而 Git 属于分布式版本控制系统。关于两者区别自行搜索资料了解下。当然学习起来，Git 相对更难上手些。

如果说准备学习 Git，我觉得结合 GitHub 来学习是很好的。说到这，或许还有人不知道 GitHub 是什么，没关系，我来解释下，简单讲就是一个开源社区网站（也被大家叫为「同性恋社区」，因为活跃着基本都是程序员呀，而这个群体基本又都是 ♂ 捂脸.jpg）。

GitHub 汇集了全球非常多的程序大牛，包括 Linux 之父  Linus（这是他的 [GitHub地址](https://github.com/torvalds)），并且 Linux 系统的代码也是公开存放在这个网站（地址：<https://github.com/torvalds/linux>），任何人都可以查看整个源代码。对于这样一个社区网站，如果只是人人可以把代码开源在上面以让其他人可以阅读并学习这样的功能，这未免太单一了。实际，GitHub 网站有很多「好玩的」，包括允许用户追踪其他用户、组织、软件库的动态，对软件代码提出问题，发表评论等。

社群功能再多，但最重要的还是版本控制，比如 A 同学开源了某个项目代码，B、C 等同学可以先 fork A 的代码到自己的账户，再 clone，即下载下来进行阅读、修改。等修改了好代码，可以发起 Pull Request 提交 A，A 最后若是觉得代码写的没任何问题，可以同意 Pull Request 并进行代码合并。这样通过多人努力，这个项目代码将会越来越好。

但整个过程必定离不开 Git 操作。虽然可以选择 Git GUI 客户端软件使用，但我还是强烈建议一开始先学 Git 命令操作。学习 Git 命令操作能更好达到对 Git 的深层理解，之后可以考虑使用 Git GUI。

这里补充下，对于如何为别人的开源的项目贡献代码？假设你是 B 同学，其大致流程如下：

1. 先点击 A 同学的项目仓库站点的 fork 的按钮，这样的你的 GitHub 账户下也会有一个相同的仓库；
2. 然后把这个 fork 过来的仓库代码 Clone 到本地，然后你就开始对该项目代码进行修改了，觉得修改 OK 了，就可以 push 到你远程仓库(即你 GitHub 账户下 fork 来的那个仓库)，最后通过内建的“pull request”机制向项目负责人申请代码合并；
3. A 要是觉得你修改的代码没啥问题，就可以同意 pull request 了，同意之后，你的代码就合并到该项目了。这样你就是该项目的贡献者之一了。

说到这，估计还是会存在部分人对 GitHub 觉得很陌生。这里摘入网上一些文章和资料先了解和学习下吧：

系列文章： [从0开始学习 GitHub 系列 - stormzhang](https://blog.csdn.net/googdev/article/details/52787516)，推荐看完该系列文章。

如何创建项目：

- [史上最全github使用方法：github入门到精通之一](http://blog.csdn.net/chaishen10000/article/details/52444384)
- [【编程初学者】创建自己的开源项目1-创建远程代码仓库](http://blog.csdn.net/jiao_zg/article/details/56496099)

关于 GitHub 网站：
- [如何用好 github 中的 watch、star、fork](https://www.jianshu.com/p/6c366b53ea41)
- [Github上如何取消fork别人的repository ](http://www.cnblogs.com/allzy/p/5068214.html)
- [GitHub Wiki 页面的添加和设置](https://lpd-ios.github.io/2017/07/11/GitHub-Wiki-Introduction/)
- ……



# 二、Git 命令


##  2.1 认识 Git

简单讲讲我的认识：首先 Git 属于「分布式版本控制系统」，先要好好理解这个分布式与集中式的不同。

集中式的如 SVN，是有一台中央服务器（其实就是某台电脑安装了 SVN 软件），所有开发人员从自己电脑（比如 Eclipse 下安装 SVN 插件）检出项目代码，任何一人修改了代码就可以提交至中央服务器，然后其他人检出（即更新、合并了代码），这样反复重复的过程，其中包括冲突的解决等，这所有的代码操作都记录在中央服务器 SVN 中的。从中可以看出这台中央服务器的作用和重要性吧，说一个很明显的问题：万一中央服务器宕机了，你就不能提交，也不能更新代码了。

分布式的如 Git，是每个人本地维护一个版本控制管理信息，那怎么做到的呢？首先你本地需要安装 [Git](https://git-scm.com/downloads) ，这个软件安装完毕，新建目录并在该目录下执行 `git init` 就会有一个 `.git` 隐藏文件夹及内容，这个文件夹下内容维护着该目录下的项目代码情况。但怎么就分布式呢？——大概是这样的，GitHub （其实就可以理解为某台电脑/服务器）上有别人提交上去的项目代码，然后你 Clone （克隆/下载）来，你本地这份项目代码就包含 `.git` 文件夹，里面就有这个项目代码的所有的版本信息，类似的，其他任何人也可以同样 Clone 下来，也是有这样一份这个项目代码的所有的版本信息，然后你们都可以基于自己 Clone 下来的项目代码进行代码的修改了，本地会记录你的修改、提交、回滚等等代码操作信息，就算 GitHub 网站挂了也没事，你们**本地都有保持着这个项目代码的所有版本控制信息。** 大概意思大家再体会下。

然后这里可以涉及到很多关于 Git 的操作，还有一些概念，比如分支。我简单说下分支，以某个 Android 项目为例，比如该项目有个主分支 master 是专门用来对外发布上线的代码，但是开发过程中某个节点遇到某个 Bug 需要修复，则可以在此开发节点新建一个比如 hotfix 分支来进行代码的修复，修复好了再合并到主分支 master 上，然后可以删除掉 hotfix 分支。

可以看出 Git 命令是学习的重点，要学的深刻，最好懂得原理和本质。本文仅是个人的学习记录，我把常用的命令整理和记录在此，方便以后查找。

## 2.2 操作本地库常用 Git 命令

1、`git init`

初始化一个目录，其实初始化完毕然后本地多出了一个 `.git`的隐藏目录，这个目录管理着一个代码库的版本信息。

2、`git add`

把一个文件从`untracked`（未被追踪）状态转为到 `staged`状态，直白的讲，就是把文件提交到`暂缓区`，这个时候还没真正意义上的代码提交。格式为：`git add .`提交所有改动，`git add hello.txt`提交指定文件的改动。

3、`git commit` 

这步才是真正的代码提交到仓库，格式为：`git commit`或者加参数`git commit -m “这次的提交说明信息”`，前者会进入一个页面，输入 i 可以进入编辑界面，再写上这次的提交的注释说明信息（一般用来记录本次提交的主要意图），然后按 ESC 键退出编辑返回到命令模式，然后连续输入两个大写的 "Z"（用 Shift 键或 Capslock 键都可以），就保存并退出了；后者的话直接可以写上提交的注释说明信息。

> 如果在提交的时候出现提示设置邮箱和用户名，是为了保证提交的准确性，在提交的时候 user.name 和 user.email 会进入日志，这些信息，是追踪代码变更的关键，比如是谁修改的。以后会随更新内容一起被永久纳入历史记录。
>
> PS：在设置用户名的时候，可以使用任意的字符。Git 实际上是使用 email 进行关联每次提交的，只不过使用 username 作为标示符来进行显示。当你的 email 地址和 github上的 email 地址一致时，则会使用 Github 上面的 name 来进行显示。
>
> 如果工作中只涉及一个 git 服务器，用一个全局配置就可以了。
>
> **全局配置：**
>
> ```
> git config --global user.name "strivebo"
> git config --global user.email "ishuzb@gmail.com"
> ```
>
> **非全局配置，某个项目下的配置：**(去掉`--global`)
>
> ```
> git config user.name "strivebo"
> git config user.email "ishuzb@gmail.com"
> ```
>
> **可以使用命令来查看修改后的配置：**
>
> ```
> git config --global user.name 或 git config user.name
> git config --global user.email 或 git config user.email
> ```
>
> **取消全局配置：**
>
> ```
> git config --global --unset user.name
> git config --global --unset user.email
> 
> git config --global user.name    #(查看)全局配置账户是否已经移除
> git config --global user.email   #(查看)全局配置邮箱是否已经移除
> ```

4、`git reset --hard` 

版本回退操作，比如我想把当前的版本回退到上一个版本，要使用什么命令呢？可以使用如下 2 种命令，第一种是：`git reset --hard HEAD^`。那么如果要回退到上上个版本只需把 `HEAD^` 改成 `HEAD^^` 以此类推。那如果要回退到前100个版本的话，使用上面的方法肯定不方便，我们可以使用下面的简便命令操作：`git reset --hard HEAD~100` 即可。

> 假设： 我进行了两次修改，第一次 readme.txt 文件添加了 2222，第二次添加了 3333，我已经使用回退操作回到了第一次的修改，即现在文本内容为 2222，但我其实又想回到第二次的修改，该怎么办呢（如何恢复 3333 内容呢）？可以这样：
>
> 通过如下命令即可获取到版本号：`git reflog`，可以看到增加内容 3333 的版本号是多少比如为 6fcfc89，我们现在可以命令：`git reset --hard 6fcfc89` 来恢复了。	

5、`git status` 

查看仓库文件状态。可以加参数 -s，即`git status -s`，加个 -s 用简洁模式查看当前修改和仓库里面差别多少，可以看到有多少文件被新增了，多少被修改了，多少被删除了。

6、`git log` 

查看提交历史记录，即版本历史信息，比如谁提交的，什么时间啊。

7、`git diff`

可以显示工作目录和暂存区之间的不同（不加选项参数）。换句话说，这条指令可以让你看到「如果你现在把所有文件都 add，你会向暂存区中增加哪些内容」。比如`git diff develop`，查看当前版本和 develop 分支的差异。 

8、`git diff –cached` 

查看已经暂存起来的文件和上次提交的版本之间的差异。`git diff –cached filename` 查看已经暂存起来的某个文件和上次提交的版本之间的差异。

9、`git diff --staged` 

使用 `git diff --staged` 可以显示暂存区和上一条提交之间的不同。换句话说，这条指令可以让你看到「如果你立即输入 git commit，你将会提交什么」。

10、`git branch` 

查看有哪些分支，并且能看到当前处于哪个分支上。注：初始化仓库后默认有 master 这个主分支，一般情况下不会轻易在该主分支操作。新建分支可以使用`git branch <newBranch>`格式，如 `git branch dev`新建分支 dev，其内容和和主分支一模一样。

11、`git branch -a`：查看本地和远程所有分支

12、`git branch -r`：查看远程所有分支

13、`git branch -v`：查看远程分支详细信息

14、`git checkout a`： 切换到 a 分支。

15、`git checkout -b a`： 

有人就说了，我要先新建再切换，未免有点麻烦，有没有一步到位的，有的：`git checkout -b a` 表示新建分支 a 并同时切换到分支 a。

16、`git merge`： 

合并分支代码，比如合并 dev 分代码，需要先切换到 master 分支，再`git merge dev`即可合并 dev 分支代码。

17、`git merge -- about` 

会尝试恢复到你运行合并前的状态。 但当运行命令前，在工作目录中有未储藏、未提交的修改时它不能完美处理，除此之外它都工作地很好。由于现在 Git 仓库处于冲突待解决的中间状态，所以如果你最终决定放弃这次 merge，也需要执行一次 merge --abort 来手动取消它。输入这行代码，你的 Git 仓库就会回到 merge 前的状态。

18、`git branch -d` 

删除分支。 假如这个分支新建错了，或者a分支的代码已经顺利合并到 master 分支来了，那么 a 分支没用了，需要删除，这个时候执行 `git branch -d a` 就可以把a分支删除了。

19、`git branch -D`：

强制删除。有些时候可能会删除失败，比如如果 a 分支的代码还没有合并到 master，你执行 `git branch -d a` 是删除不了的，它会智能的提示你 a 分支还有未合并的代码，但是如果你非要删除，那就执行 `git branch -D a` 就可以**强制删除** a 分支。

20、`git tag`

新建标签。我们在客户端开发的时候经常有版本的概念，比如 v1.0、v1.1 之类的，不同的版本肯定对应不同的代码，所以我一般要给我们的代码加上标签（即把某次提交标记为某个 tags，如 v1.0），这样假设 v1.1 版本出了一个新 bug，但是又不晓得 v1.0 是不是有这个 bug，有了标签就可以顺利切换到 v1.0 的代码，重新打个包测试了。所以如果想要新建一个标签很简单，比如 `git tag v1.0` 就代表我在当前代码状态下新建了一个 v1.0 的标签，输入 git tag 可以查看历史 tag 记录。 想要切换到某个 tag，执行：`git checkout v1.0`，就可以切换到 v1.0 的代码状态。

21、`git branch -vv`

查看本地分支关联（跟踪）的远程分支之间的对应关系，本地分支对应哪个远程分支。

22、`git push origin v0.1.2 `

表示将 v0.1.2 标签提交到 Git 服务器（通常的 `git push` 不会将标签对象提交到 Git 服务器，我们需要进行该显式操作）。如果将本地所有标签一次性提交到 Git 服务器，可以 `git push origin –tags`。

22、合并多次 commit，参考如下

``` 
git rebase -i HEAD~4  #可以看到最近 4 次的 commits，然后修改 commit 前面的为 squash，改完后保存，后继也会出现可以填写该次的 message
git add .
git rebase --continue 
git push -f 	#  -f 强制推送到远程服务器
```

## 2.3 操作远程库相关 Git 命令

1、`git clone` 

远程 clone 即复制/克隆一个完整的 repository (仓库，即项目代码)到本地，克隆仓库时所使用的远程主机自动被 Git 命名为 `origin`，如果想用其他的主机名，需要用`git clone`命令的`-o`选项指定。格式为：`git clone -o jQuery https://github.com/jquery/jquery.git`，然后`git remote`命令查看，可以看到名字为远程主机名 jQuery。

> 这里要特别说下，这里克隆可以有两种方式，一种 https 方式，一种 ssh 。
>
> ①如果是 https 方式，复制仓库 https 地址进行 clone 操作，如：`git clone https://github.com/strivebo/git-practice.git`
>
> 这样克隆下来的项目仓库，注意观察`.git`文件夹下的`config`中的文件 url：
>
> ```
> [remote "origin"]
> 	url = https://github.com/strivebo/git-practice.git
> 	fetch = +refs/heads/*:refs/remotes/origin/*
> [branch "master"]
> 	remote = origin
> 	merge = refs/heads/master
> ```
>
> ②如果是 ssh 方式，复制仓库的 ssh 地址进行 clone 操作，如：`git clone git@github.com:strivebo/git-practice.git`
>
> 这样克隆下来的项目仓库，注意观察`.git`文件夹下的`config`中的文件 url：
>
> ```
> [remote "origin"]
> 	url = git@github.com:strivebo/git-practice.git
> 	fetch = +refs/heads/*:refs/remotes/origin/*
> [branch "master"]
> 	remote = origin
> 	merge = refs/heads/master
> ```
>
> **注1：** 可以看到，https 方式下 url 为 「https 地址」，ssh 方式下 url 为「ssh 地址」（我就这么任性表达了，反正意思明白就行），所以假设你采用的 https 方式 clone 下来的项目可以通过修改这个 url 为「ssh 地址」，这样本地仓库就相当于是使用了 「ssh 方式 clone 下来的」。
>
> **注2：**两者的区别有，若采用的 https 方式，则每次提交代码至 GitHub 时，都要求输入 GitHub 账号和密码才能提交，若 ssh 方式，则不需要每次的输入。但当然这前提是你已经添加 ssh 。
>
> 关于 SSH 协议的历史，可以看看这篇文章：[SSH 协议（Secure Shell 协议）](http://www.cnblogs.com/ifantastic/p/3984544.html)。 
>
> 这里引用我看到的网上资料关于 https 和 SSH 的区别说下：
>
> 1. 前者可以随意克隆 github上的项目，而不管是谁的；而后者则是你必须是你要克隆的项目的拥有者或管理员，且需要先添加 SSH key ，否则无法克隆。
> 2. https url 在 push 的时候是需要验证用户名和密码的；而 SSH 在 push 的时候，是不需要输入用户名的，如果配置 SSH key 的时候设置了密码，则需要输入密码的，否则直接是不需要输入密码的。
>
> **关于如何添加 ssh 下面是步骤：** 
>
> 1. Linux 与 Mac 都是默认安装了 SSH ，而 Windows 系统安装了 Git Bash（即安装了 Git 就有这个） 应该也是带了 SSH 的，在终端输入`ssh`命令可以查看是否安装了 ssh；
>
> 2. 紧接着输入 `ssh-keygen -t rsa` 或者`ssh-keygen -t rsa -C "注释"`格式 ，就是指定 rsa 算法生成密钥，接着连续三个回车键（不需要输入密码）然后就会生成两个文件 id_rsa 和 id_rsa.pub ，而 id_rsa 是密钥，id_rsa.pub 就是公钥。这两文件默认分别在如下目录里生成： Linux/Mac 系统 在 `~/.ssh` 下，win系统在 `/c/Documents and Settings/username/.ssh` 下， 都是隐藏目录，大家应该能找到的；
>
> **注：** 其实在连续安回车键中会提示输入一个密码以及确认密码，这个密码会在你提交项目时使用，如果为`空`的话（即直接按回车键，也即未设置密码）提交项目代码时则不用输入密码；
>
> 3. 接下来要做的是把 id_rsa.pub 的内容添加到 GitHub 上（PS：如何添加自行网上搜下，就不多说了），这样你本地的 id_rsa 密钥跟 GitHub 上的 id_rsa.pub 公钥进行配对，授权成功，这样就可以不用像 https 方式每次输入账号和密码进行验证身份才能提交了。（你就理解为，SSH 就好比进行了身份验证的这种理解。）
>
> 4. SSH key 添加成功之后，输入 `ssh -T git@github.com` 进行测试，如果出现以下提示，再输入 yes 出现如下图则证明添加成功了。（图我就不截了，我觉得问题应该不大）
>
> 补充：对于命令 `ssh-keygen`添加不同参数的含义—— [ ssh-keygen参数说明](http://blog.csdn.net/u011686226/article/details/52412393)。
>

2、`git remote` 

列出所有的远程仓库。从别处 clone 来的，默认都会有一个别名”origin”的仓库。带上 -v 可以看到具体 URL。

3、`git remote add` 

添加远程仓库地址。其实这些操作都是在本地，并没有实际牵涉到远程。另外 github 里面fork 过来的，默认叫”upstream”。该命令完整格式为：`git remote add <主机名> <网址>`，如`git remote add orgin git@github.com:strivebo/git-practice.git`

4、`git remote rw`

删除远程仓库地址。格式为：`git remote rm <主机名>`。

5、`git remote rename`： 

用于远程主机的改名。完整格式为：**`git remote rename <原主机名> <新主机名>`** 。

6、`git fetch`

一旦远程主机的版本库有了更新（Git 术语叫做 commit），需要将这些更新取回本地，这时就要用到`git fetch`命令。格式为：`git fetch <远程主机名>`，默认情况下，`git fetch`取回所有分支（branch）的更新。

- 如果只想取回特定分支的更新，可以指定分支名，格式为：**`git fetch <远程主机名> <分支名>`**， 另外，所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取，比如`origin`主机的`master`，就要用`origin/master`读取。
- 取回远程主机的更新以后，可以在它的基础上，使用`git checkout`命令创建一个新的分支，`git checkout -b newBrach origin/master`，该命令表示，在`origin/master`的基础上，创建一个新分支。此外，也可以使用`git merge`命令或者`git rebase`命令，在本地分支上合并远程分支。

7、`git pull`

`git pull`命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。相当于 fetch后，再进行 merge。其完整格式为：**`git pull <远程主机名> <远程分支名>:<本地分支名>`**，如取回`origin`主机的`next`分支，与本地的`master`分支合并，可以这样写：`git pull origin next:master`

- 如果远程分支是与当前分支合并，则冒号后面的部分可以省略，即`git pull origin next`，该命令表示，取回`origin/next`分支，再与当前分支合并。实质上，这等同于先做`git fetch`，再做`git merge`

在某些场合，Git会自动在本地分支与远程分支之间，建立一种追踪关系（tracking）。比如，**在`git clone`的时候，所有本地分支默认与远程主机的同名分支，建立追踪关系，也就是说，本地的`master`分支自动"追踪"`origin/master`分支。** Git也允许手动建立追踪关系。

- `git branch --set-upstream master origin/next`该命令指定`master`分支追踪`origin/next`分支

- 如果当前分支与远程分支存在追踪关系，`git pull`就可以省略远程分支名。`git pull origin`该命令表示，本地的当前分支自动与对应的`origin`主机"追踪分支"（remote-tracking branch）进行合并

- 如果当前分支只有一个追踪分支，连远程主机名都可以省略，`git pull`该命令表示，当前分支自动与唯一一个追踪分支进行合并

- 合并需要采用rebase模式，可以使用`--rebase`选项。`git pull --rebase <远程主机名> <远程分支名>:<本地分支名>`

- 如果远程主机删除了某个分支，默认情况下，`git pull` 不会在拉取远程分支的时候，删除对应的本地分支。这是为了防止，由于其他人操作了远程主机，导致`git pull`不知不觉删除了本地分支。但是，你可以改变这个行为，加上参数 `-p` 就会在本地删除远程已经删除的分支。`git pull -p` 该命令等同于：

  ``` 
  git fetch --prune origin 
  git fetch -p
  ```

8、`git push`

`git push`命令用于将本地分支的更新，推送到远程主机。它的格式与`git pull`命令相仿。其完整格式为：**`git push <远程主机名> <本地分支名>:<远程分支名>`** 

- 如果省略远程分支名，则表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，则会被新建。`git push origin master`该命令表示，将本地的`master`分支推送到`origin`主机的`master`分支。如果后者不存在，则会被新建。
- 如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。`git push origin :master`等同于`git push origin --delete master`该命令表示删除`origin`主机的`master`分支。
- 如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。`git push origin`该命令表示，将当前分支推送到`origin`主机的对应分支。
- 如果当前分支只有一个追踪分支，那么主机名都可以省略。`git push`
- 如果当前分支与多个主机存在追踪关系，则可以使用`-u`选项指定一个默认主机，这样后面就可以不加任何参数使用`git push`。`git push -u origin master`该命令将本地的`master`分支推送到`origin`主机，同时指定`origin`为默认主机，后面就可以不加任何参数使用`git push`了。

注：不带任何参数的`git push`，默认只推送当前分支，这叫做 simple 方式。此外，还有一种matching 方式，会推送所有有对应的远程分支的本地分支。Git 2.0 版本之前，默认采用 matching 方法，现在改为默认采用 simple 方式。如果要修改这个设置，可以采用`git config`命令。

``` 
$ git config --global push.default matching
# 或者
$ git config --global push.default simple
```

还有一种情况，就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要使用`--all`选项。

```
$ git push --all origin
```

上面命令表示，将所有本地分支都推送到`origin`主机。

如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做`git pull`合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用`--force`选项。

``` 
git push --force origin
```

上面命令使用`--force`选项，结果导致远程主机上更新的版本被覆盖。除非你很确定要这样做，否则应该尽量避免使用`--force`选项。

最后，`git push`不会推送标签（tag），除非使用`--tags`选项。`git push origin --tags`。

9、删除远程分支：`git push origin --delete <branchName>`

否则，可以使用这种语法，推送一个空分支到远程分支，其实就相当于删除远程分支：`git push origin :<branchName>`。

10、删除 tag：`git push origin --delete tag <tagname>`

另外，这也是删除 tag 的方法，推送一个空 tag 到远程tag：`git tag -d <tagname>`、`git push origin :refs/tags/<tagname>`。

11、修改还未 push 的注释：`git commit --amend`，修改后保存退出。刚刚 push 到远端还没有人其他人下载或改动的：

```xml
git commit --amend
```

进入修改页面修改注释信息，修改后 :wq 保存退出。 再使用`git push --force-with-lease origin master`。如果其他人已经下载或改动：

```xml
git fetch origin
git reset --hard origin/master
```

来源：[git修改未push和已经push的注释信息](https://blog.csdn.net/yulsh/article/details/54613189)

## 2.4 参考资料

- [Git远程操作详解](http://www.ruanyifeng.com/blog/2014/06/git_remote.html) - 阮一峰网络日志
- [Git 原理详解及实用指南 - 掘金小册](https://juejin.im/book/5a124b29f265da431d3c472e/section) - 抛物线。
- [Git--版本控制](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%85%B3%E4%BA%8E%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6) - 官方文档。
- [Git常用操作小结](https://github.com/muwenzi/Program-Blog/issues/13)
- [Git的点点滴滴，结合了部分Android Studio自带的版本控制功能](http://blog.csdn.net/sxh951026/article/details/77200003)
- [git 有用却易忘的知识与命令](https://www.zybuluo.com/yangfch3/note/159758)



# 三、实战学习

## 3.1 代码提交到 GitHub 上

### 1) 两种克隆 Clone 方式

1、采用 HTTPS 方式克隆 GitHub 上仓库(项目)：`git clone https://github.com/strivebo/git-practice.git`

2、采用 SSH 方式克隆：`git clone git@github.com:strivebo/git-practice.git`

- 如果想要在克隆至本地时指定别的目录名称，可以在后面加个参数，如：`git clone https://github.com/strivebo/git-practice.git git-practice-another`，手动指定本地仓库的根目录名称为 git-practice-another。

提下，GitHub 中的 SSH 和 HTTPS 提交区别：[github中ssh和https提交的区别](http://blog.csdn.net/itzhongzi/article/details/62250419)  | [git使用ssh密钥和https两种认证方式汇总（转） ](http://www.cnblogs.com/softidea/p/5448118.html)。

解释下：从 GitHub 上 Clone 一个项目到本地的时候，有 use https 和 use ssh 两种方式，这两种主要是在 push 项到 GitHub 上时有所不同。完成一个 push 操作，需要对其内容进行安全管理，这里提供了 ssh 和 https 两种方式。而在 Clone 项目到本地时，做出选择后，就已经决定了 push 的方式。

SSH 使用了 RSA，即非对称加密的方式，存在一个公钥和私钥。可以生成一个本地的一组秘钥，然后将公钥复制到 GitHub 的 settings/profile 下。使用 https 方式，每次需要验证用户身份信息。

### 2) 采用 https 方式克隆

在使用 `git status` 命令查看仓库状态若是看到： `your branch is ahead of 'origin/master' by 2 commits.` ，解释下：

1. Git 提示你的当前 branch 已经领先于（ "ahead of" ）'origin/master' 两个提交了
2. origin/master 的中的 origin 是远端仓库的名称，是你在用 Clone 指令初始化本地仓库时 Git 自动帮你起的默认名称；master 是 origin 上的分支名称。

可以暂时把 origin/master 简单理解为「中央仓库」，也就是说，这句话是告诉你，你的本地仓库已经领先中央仓库两个提交了。然后可以使用 `git push` 提交发布至中央服务器（这里即指 GitHub）。

因为采用的是 https 方式克隆，所以在这个过程 GitHub 会向你索要账户和密码。填入正确的账户和密码，push 操作就完成了。这时你再去你的 GitHub 仓库页面可以看到提交记录。说明你已经成功把本地仓库的提交推送到了服务器了。

PS：如果觉得一遍遍地输入密码很烦，可以按照 [这个页面](https://help.github.com/articles/caching-your-github-password-in-git/) 提供的方案来把密码保存起来。另外还有一个更简单但安全性低一些的方案。执行这行代码：`git config credential.helper store`，在这之后你只需要再输入一次密码， Git 就会把你的密码保存下来，这之后就再也不用输入了。说它「安全性低」，是因为这条指令会让 Git 把你的密码以明文形式保存在你的电脑上。具体这两种保存密码的方案选择哪个，看你自己了。

总结下：

1. 从 GitHub 把中央仓库 Clone 到本地把写完的代码提交。即先使用命令：`git clone`，再用 `git add 文件名` 把文件添加到暂存区，再用 `git commit` 提交；
2. 在这个过程中，可以使用 `git status` 来随时查看工作目录的状态。每个文件有 "changed / unstaged"（已修改）, "staged"（已修改并暂存）, "commited"（已提交） 三种状态，以及一种特殊状态 "untracked"（未跟踪）；
3. 提交一次或多次之后，把本地提交 push 到中央仓库（命令：`git push`）。


### 3) 采用 ssh 方式克隆

在拥有了一个 GitHub 账号之后，就可以自由的 Clone 或者下载其他项目，也可以创建自己的项目，但是你没法提交代码。仔细想想也知道，肯定不可能随意就能提交代码的，如果随意可以提交代码，那么 GitHub 上的项目岂不乱了套了，所以提交代码之前一定是需要某种授权的，而 GitHub 上一般都是基于 SSH 授权的。那么什么是 SSH 呢？ 简单点说，SSH 是一种网络协议，用于计算机之间的加密登录。目前是每一台 Linux 电脑的标准配置。而大多数 Git 服务器都会选择使用 SSH 公钥来进行授权，所以想要在 GitHub 提交代码的第一步就是要先添加 SSH key 配置。

添加 SSH 步骤：
1. Git Bash下输入 `ssh` 查看电脑是否安装了 ssh；
2. 紧接着输入 `ssh-keygen -t rsa`，什么意思呢？就是指定 rsa 算法生成密钥，接着连续三个回 
    车键（不需要输入密码） ，然后就会生成两个文件 `id_rsa` 和 `id_rsa.pub` ，而 id_rsa 是密钥，id_rsa.pub 就是公钥。这两文件默认分别在如下目录里生成： Linux/Mac 系统 在 ~/.ssh 下，Windows 系统在 /c/Documents and Settings/username/.ssh 下， 都是隐藏文件。
3. 接下来要做的是把 id_rsa.pub 的内容添加到 GitHub 上，这样你本地的 id_rsa 密钥跟 GitHub 上的 id_rsa.pub 公钥进行配对，授权成功才可以提交代码。
4. SSH key 添加成功之后，输入 `ssh -T git@github.com` 进行测试。

最后就是 push、pull 的操作了。添加 SSH key 成功之后，我们就有权限向 GitHub 上我们自己的项目提交代码了。执行：`git push origin master` 进行代码提交。

**实践：** 假设我们本地有个 test2 的项目，我们需要的是在 GitHub 上建一个 test 的项目，然后把本地 test2 上的所有代码 commit 记录提交到 GitHub 上的 test 项目。

1. 第一步就是在 GitHub 上建一个 test 仓库，具体怎么操作我就不多说了吧；

2. 第二步切换到 test2 目录，打开 Git Bash，把本地 test2 项目与 GitHub 上的 test 项目进行关联：`git remote add origin git@github.com:strivebo/test.git`；

   什么意思呢？就是添加一个远程仓库，地址是 `git@github.com:strivebo/test.git`，而 origin 是给这个项目的远程仓库起的名字，是的，名字你可以随便取，只不过大家公认的只有一个远程仓库时名字就是 origin ，为什么要给远程仓库取名字？因为我们可能一个项目有多个远程仓库？比如 GitHub 一个，比如公司一个，这样的话提交到不同的远程仓库就需要指定不同的仓库名字了。

注：查看我们当前项目有哪些远程仓库可以执行如下命令： `git remote -v`，接下来，我们本地的仓库就可以向远程仓库进行代码提交了：`git push origin master`，就是默认向 GitHub 上的 test 仓库提交了代码，而这个代码是在 master 分支，当然你可以提交到指定的分支。

**再次强调：** Git 使用 https 协议，每次 pull，push 都要输入密码，相当的烦。使用 Git 协议，然后使用 ssh 密钥，这样可以省去每次都输密码。



# 四、问题和笔记

## 4.1 问题

### 1. fatal the current branch master has no upstream branch

对于前面这个「假设我们本地有个 test2 的项目，我们需要的是在 GitHub 上建一个 test 的项目，然后把本地 test2 上的所有代码 commit 记录提交到 GitHub 上的 test 项目。」实践练习有出现了问题，报错是：fatal the current branch master has no upstream branch.

参考网上资料：

- [git：fatal the current branch master has no upstream branch](http://blog.csdn.net/qqb123456/article/details/25319659)
- [在GitHub上管理项目](http://www.cnblogs.com/mengdd/p/3447464.html)
- [git push 操作](http://blog.csdn.net/xhl_will/article/details/8442546)

我的总结：如果没有添加 ssh，没采用 ssh 方式克隆，那采用 https 方式克隆，如：`git remote add origin https://github.com/strivebo/test.git`，然后，下面是引用的网上一个人的解决方式：

> 此时如果 origin 的 master 分支上有一些本地没有的提交，push 会失败。所以解决的办法是，首先设定本地 master 的上游分支:`git branch --set-upstream-to=origin/master`，然后 pull：`git pull --rebase`，最后再 push：`git push`。
>

### 2. 官网下载的 Git 与 TortoiseGit 客户端的关系？

Git 自带GUI界面。使用 `git gui` 命令可以打开它。在这个界面中可以完成 commit、merge、push、pull 等等常用操作。

.......

TortoiseGit 没有集成 Git。在 [TortoiseGit](http://code.google.com/p/tortoisegit/) 官方网站可以下载到它。有 32bit 和 64bit 版本，同时也有中文语言包（但我不建议你安装）。安装完毕之后，如果你没有安装过 Git，那么还需要去下载 [msysGit](http://msysgit.github.com/) 来安装。因为 TortoiseGit 其实只是一个壳，它需要调用 Git 命令行才能发挥作用。（现在你知道我为什么推荐你用命令行了么？）

为什么 TortoiseGit 不像 TortoiseSVN 一样，把 SVN 命令行工具集成在安装包中呢？我猜想是以下几点原因：

- Git 官方从未出过 Windows 版本二进制包；
- msysGit 和 TortoiseGit 是两个不同的团队开发的；
- msysGit 和 TortoiseGit 的更新周期差异较大；
- TortoiseGit 团队希望安装包更小；
- TortoiseGit 团队给用户更灵活的选择 Git 版本的权利。

来源：[使用Git、Git GUI和TortoiseGit](https://blog.zengrong.net/post/1722.html#)

### 3. Git 如何 Clone 非 master 分支代码

问题描述：在从 GitHub 上 Clone 项目下来的时候，如 https 方式克隆某个具有多个分支的项目：`git clone https://github.com/TeamNewPipe/NewPipe.git` 	

注：该分支默认分支为 dev 分支，其他分支有 master 、multyservice 分支。

出现的问题是：克隆完毕，使用 `git branch` 查看本地分支，只能看到 dev 分支。如果想要是查看或是说克隆非默认分支代码，如何操作呢？以下两种解决方式供参考：

①第一种：

新的解决方法：先 `git branch -a` 列出本地和远程所有分支，比如某个远程分支为 origin/daily/1.4.1，然后再直接使用 `git checkout origin/daily/1.4.1`

旧的解决方法：1、先在本地建立与远程分支同名分支名称；2、切换到该本地分支；3、建立上游分支，即 `git branch --set-upstream-to=origin/daily/1.4.1 daily/1.4.1`，这样完成与上游分支的关联，然后 pull 就好了。

参考：[Git 如何 clone 非 master 分支的代码](https://gaohaoyang.github.io/2016/07/07/git-clone-not-master-branch/)

②第二种：

Git 默认只显示默认分支的数据，需要手动切换到我们需要的分支并显示出来。

```
git branch
git checkout -b <本地分支名字> origin/<远程分支名字>
```

这样大功告成。

参考：[克隆Github上项目的非Master分支](https://www.jianshu.com/p/79cdb8d514ed)

亲测第二种方式是可以的。

### 4. git pull 和 git fetch 有什么区别？

首先，你的每一个操作都是要指明【来源】和【目标】的，而对于 pull 来说，【目标】就是当前分支；

其次，你得清楚 Git 是有 tracking 的概念的，所谓 tracking 就是把【来源】和【目标】绑定在一起，节省一些操作是需要输入的参数。

那么，假设你的 master 和 develop 都是 tracking 了的，于是：

- 当你在 master 下，`git pull`等于 fetch origin，然后 merge origin/master
- 当你在 develop 下，`git pull`等于 fetch origin，然后 merge origin/develop

参考：[git pull 和 git fetch 有什么区别？](https://ruby-china.org/topics/15729)



## 4.2 笔记 

### 1. 在本地仓库初始化后，不进行 commit 提交，则新建不了分支；进行了commit提交，则真正建立了 master 分支

在某些场合，Git 会自动在本地分支与远程分支之间，建立一种追踪关系（tracking）。比如，在 git clone 的时候，所有本地分支默认与远程主机的同名分支，建立追踪关系，也就是说，本地的 master分支自动"追踪" origin/master 分支。Git 也允许手动建立追踪关系：`git branch --set-upstream master origin/next` 该命令指定 master 分支追踪 origin/next 分支。

——*来自阮一峰老师的 Git 文章。* 

### 2. 重命名本地和远程分支名称

在 Git 中重命名远程分支，其实就是先删除远程分支，然后重命名本地分支，再重新提交一个远程分支。

1. 删除分支的命令是：在 Git1.7 之后可以使用这个命令 `git push origin --delete <远程分支名称> `，否则用这个也可以：`git push origin :<远程分支名称>`表示推送一个空分支到远程分支，其实就相当于删除远程分支；
2. 重命名本地分支：`git branch -m <旧名称> <新名称>`；
3. 重新提交：`git push origin <新名称>:<新名称>`。



# 五、Git 图形化客户端 SourceTree

SourceTree 是 Windows 和 Mac OS X 下免费的 Git 和 Hg 客户端，拥有可视化界面，容易上手操作。同时它也是 Mercurial 和 Subversion 版本控制系统工具。支持创建、提交、clone、push、pull 和 merge 等操作。

SourceTree 官方下载：[传送门](https://www.sourcetreeapp.com/)。

Sourcetree 可简化 Mercurial 和 Git 存储库的交互，让我们集中精力编写代码。通过 Sourcetree 简单的 Git 图形用户界面查看和管理您的存储库。

- 非常简单，适合初学者：告别命令行 - 通过 Git 客户端简化分发版本的控制，快速为每个人提供最新信息。
- 让专家如虎添翼：非常适合用于提高高级用户的工作效率。查看分支之间的变更集、stash、cherry-pick 等等。
- 可视化代码：眼见真的为实。单击一次即可获取有关所有分支或提交的信息。
- 桌面上的 Git 和 Hg：功能完善的图形用户界面，开箱即用，可提供高效、一致的开发流程。可与 Git 和 Mercurial 搭配使用。

网上的教程：

- [图解GitHub和SourceTree入门教程](http://blog.csdn.net/collonn/article/details/39259227)
- [SourceTree+Git简单使用(Windows)](http://blog.csdn.net/u012792686/article/details/63684248)



---

*update：2019-07-21*