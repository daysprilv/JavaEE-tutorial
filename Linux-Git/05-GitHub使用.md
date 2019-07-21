# 一、GitHub 使用技巧

## 1. 突破GitHub单个大文件上传限制

GitHub 上新建的仓库容量大小限制在 1G，单个文件不能超过 100M，有 50M 的文件，就会警告了。 可通过以下命令查找超过 100M 的文件：`find ./ -type f -size +102400k`。 想要突破 GitHub 的限制，支持单个文件超出 100M，可以使用  [Git LFS](https://github.com/git-lfs/git-lfs)。 

参考：[突破github的100M单个大文件上传限制 ](https://blog.csdn.net/tyro_java/article/details/53440666)

## 2. Git只Clone仓库指定文件或文件夹

步骤总结如下：

``` html
git init <repo> 	#新建仓库并初始化
cd <repo> 	#切换到该仓库目录
git remote add origin <url>		#拉取remote的all objects信息，url为仓库地址
git config core.sparsecheckout true		#设置允许克隆子目录，即开启sparse clone
echo "参考论*" >> .git/info/sparse-checkout	#设置需要pull的目录，*表示所有，!表示匹配相反的
git pull --depth=1 origin master	#将origin端指定目录下的文件pull到本地
```

通过本人亲自实践，有几点说的：

1. 像这步命令`echo "参考论" >> .git/info/sparse-checkouth `会在`.git/info/`目录下生成`sparse-checkout`无后缀文件，打开看到内容可以看到设置为了`参考论`，即会下载匹配`参考论`的目录下所有文件。也可以修改为别的，比如修改诸如为`参考论文/files/101`，匹配`参考论文/files/101`文件或文件夹？。

2. 通过实践，发现其实不管如何设置下载指定的文件或文件夹，其实它都是有下载整个仓库容量大小，正如知乎[宫玖的回答 - 知乎](https://www.zhihu.com/question/21114773/answer/34943423)，@依云指出：

   > 你一定没有亲自去尝试这个功能。「checkout」!=「clone/fetch」。实际上整个仓库还是会全部拖回本地，只是检出的时候不检出别的目录而已。

3. `git pull --depth=1 origin master` ：depth 用于指定克隆深度，为 1 即表示只克隆最近一次 commit，可以不加，否则可能出现问题。补充： [git clone 时使用了 --depth 后，如何再重新拉取全部的历史](https://mozillazg.com/2016/01/git-revert-depth-1.html)。

PS：后来发现，其实直接打开想要下载的文件，在右上角可以直接鼠标右键点击`Raw`选择链接另存为即可下载。或是使用浏览器插件【[GitZip for github](https://chrome.google.com/webstore/detail/gitzip-for-github/ffabmkklhbepgcgfonabamgnfafbdlkn) 】可以非常方便下载，推荐。

参考：

- [git clone克隆或下载一个仓库单个文件夹](https://blog.csdn.net/qq_36560161/article/details/78260532)
- [git只clone仓库中指定子目录和指定文件的实现](http://www.cnblogs.com/juking/p/7223669.html)

## 3. 利用GitHub进行多人协作开发

### (1) 开发并且提交代码

首先要从 GitHub 上 clone 自己仓库代码到本地，你需要执行如下命令：

``` xml
# 如果没有配置ssh，可以 git clone https://github.com/strivebo/git-practice.git
git clone git@github.com:strivebo/git-practice.git 
```

然后代码下载到本地了，修改代码，然后可以提交代码，命令如下：

``` xml
git add .	# 表示提交所有改动，指定提交某个文件的改动，则可 git add hello.txt
git commit  -m '修改原因，相关说明信息'
```

执行`git commit`之后，只是提交到了本机的仓库，而不是 GitHub 上你账号的仓库。你需要执行 push 命令，把 commit 提交到服务器。

``` xml
# git push #直接到远程默认仓库，或者下面这个：
git push orgin master #push到名为orgin的远程仓库的指定分支master
```

这样就完成了修改远程仓库代码了。

### (2) 多人协作开发

Q：假如想要进行多人协作开发。比如要对 xiaoming 的名为 git-test 的仓库贡献自己写的代码，比如说添加新功能，怎么操作呢？

> A：首先你需要 fork 一份 xiaoming 的 git-test 仓库到自己 GitHub 仓库，这个时候这个仓库就是你的了，再 clone 到本地磁盘，然后按上小节流程操作就可以完成对自己 fork 来的远程 git-test 仓库的代码修改工作。
>
> 然后可以发起 pull request 给 xiaoming 请求合并代码就行，随即 xiaoming 本人就会看到你写的代码，如果他觉得不错，没问题，他就可以进行合并了。（关于如何发起 pull request，请点击本小节参考资料查阅，有截图\~）

但这里的合作开发会有一个问题，如何与 xiaoming 的仓库代码保持同步？

因为在自己做开发的过程中，难免会遇到“Fork”的项目已经有了新的更新，这时当然是希望自己仓库中的代码也能同步进行更新。可是，你本地仓库所连接的远程仓库的是你自己的 GitHub 仓库，而不是原作者的仓库。 **解决方法其实很简单，为你的本地仓库再添加一个远程仓库源。** 步骤如下：

①先查看当前项目所连接的远程仓库：`git remote -v`，一般情况可以看到目前连接了自己的远程仓库，截图我就略了；

②然后添加源作者 xiaoming 的远程仓库连接：`git remote add upstream git@github.com:xiaoming/git-test.git`

这个命令什么意思呢？就是对本地仓库再关联一个远程连接，名称为 upstream，地址为`git@github.com:xiaoming/test.git`（如果要取消该关联，使用这个命令：`git remote rm upstream`）

③

``` xml
（1）从原仓库获取最新版本到本地
git fetch upstream master

（2）保证当前位于 master 分支上
git checkout master

（3）将最新版本整合到本地 master 分支上
git merge upstream/master

（4）将更新发送到自己的 GitHub 仓库里
git push origin master
```

或者①②③步可以用： `git pull upstream master` 这条命令替代，不过这样不太安全，因为你 fetch（获取）之后可以通过：`git log --oneline --graph --decorate --all`来查看更新的情况，再决定是否 merge（整合）到一起。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190127214003.png)

如上操作完毕，这样自己 fork 过来的仓库代码就和原作者仓库代码保证一致了。

也可参考该文：[github上fork了别人的项目后，再同步更新别人的提交](https://blog.csdn.net/qq1332479771/article/details/56087333)，该文第二种方法即上面讲的方式，命令如下：

``` xml
git remote -v 
git remote add upstream git@github.com:xxx/xxx.git
git fetch upstream
git merge upstream/master
git push 
```

参考资料：

- [如何利用 Git 与 GitHub 进行多人协作开发](https://www.jianshu.com/p/8c69d1021d98)
- [github的多人协作](https://gist.github.com/suziewong/4378619#gistcomment-1873334)

## 4. 同一台电脑配置多个Git账号

在日常使用 GitHub 作为仓库使用的时候，有时可能会遇到这样的一些情况：

``` xml
1. 有两个 github 账号，一台电脑怎么同时连接这两个账号进行维护呢？
2. 自己用一个 github 账号，平时用来更新自己的一些资料；公司使用的 gitlab（也是 git 的衍生产品）
```

SSH Key 的配置：

1. `Windows` 下打开 `Git Bash`，创建 `SSH Key`，按提示输入密码，可以不填密码一路回车

   ``` xml
   $ ssh-keygen -t rsa -C "注释" 	#如：ssh-keygen -t rsa -C “123456@qq.com”
   ```

2. 生成另外一个账号新的 SSH keys

   ``` xml
   ssh-keygen -t rsa -C "注释"	#如：ssh-keygen -t rsa -C "123456@sina.com"
   ```

   平时我们都是直接回车，默认生成 `id_rsa` 和 `id_rsa.pub`。这里特别需要注意，出现提示输入文件名的时候(`Enter file in which to save the key (\~/.ssh/id_rsa): id_rsa_new`)要输入与默认配置不一样的文件名，比如：我这里填的是 `id_rsa_new`。

   其实也可以一个命令操作，是使用 -f 参数指定文件名：`ssh-keygen -t rsa -C "注释" -f id_rsa_new"`

3. 配置 `C:\Users\用户名\.ssh\config` 文件。在 `.ssh`文件夹下新建 config 文件（无后缀名），修改如下：

   ``` xml
   #github
   Host github.com
   HostName github.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/id_rsa
   
   #github2
   Host second.github.com
   HostName github.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/id_rsa_new
   ```

   *注：令不同 Host 实际映射到同一`HostName`，但密钥文件不同。Host 前缀可自定义。*

   它们具体的含义如下：

   ``` xml
   #Host myhost（这里是自定义的host简称，以后连接远程服务器就可以用命令ssh myhost）[注意下面有缩进]
   #User 登录用户名(如：git)
   #HostName 主机名可用ip也可以是域名(如:github.com或者bitbucket.org)
   #Port 服务器open-ssh端口（默认：22,默认时一般不写此行）
   #IdentityFile 证书文件路径（如~/.ssh/id_rsa_*)
   ```

4. 测试：

   ``` xml
   $ ssh -T git@github.com
   Hi xiaoming! You've successfully authenticated, but GitHub does not provide shell access.
   
   $ ssh -T git@second.github.com
   Hi zhangsan!  You've successfully authenticated, but GitHub does not provide shell access.
   ```

解决方案总结为：

1. 生成私钥/公钥，密钥文件命名避免重复
2. 设置不同 Host 对应同一 HostName 但密钥不同
3. 取消 git 全局用户名/邮箱设置，为每个仓库独立设置用户名/邮箱

如何取消 Git 全局用户名/邮箱设置，如下：

1. 使用 `git config --list` 查看当前配置

   如果你之前在设置本地仓库和 `github` 连接的时候设置过 `user.name` 和 `user.email`, 那么你必须首先清楚掉该设置，因为不清楚掉该设置，两个账号在提交资料的时候，验证肯定冲突（只能设置一个全局的`user.name` 和 `user.email`，而你现在有两个账号就对应两个不同的）。

2. 取消 global

   ``` xml
   git config --global --unset user.name
   git config --global --unset user.email
   ```

3. 设置每个项目 repo 的自己的 user.email

   ``` xml
   git config  user.email "xxxx@xx.com"
   git config  user.name "suzie"
   ```

或者直接直接编辑电脑`.gitconfig` 文件（Windows 系统在`C:\Users\用户名\.gitconfig`目录），把 `name` 和 `email` 都去掉，从而取消全局用户/邮箱设置。

参考资料：

- [一台电脑绑定两个github帐号教程](https://www.jianshu.com/p/3fc93c16ad2d)
- [同一台电脑配置多个git账号](https://github.com/jawil/notes/issues/2)
- [一台电脑绑定两个git帐号(GitHub和GitLab)](https://blog.csdn.net/jifaliwo123/article/details/79126785)

## 5. Git配置多个SSH-key？为什么？

背景：当有多个 git 账号时，比如：

``` xml
a. 一个 gitee，用于公司内部的工作开发；
b. 一个 github，用于自己进行一些开发活动；
```

操作步骤：

①生成一个公司用的 SSH-Key

``` xml
ssh-keygen -t rsa -C 'xxxxx@company.com' -f ~/.ssh/gitee_id_rsa
```

②生成一个 github 用的SSH-Key

``` xml
ssh-keygen -t rsa -C 'xxxxx@qq.com' -f ~/.ssh/github_id_rsa
```

③在 \~/.ssh 目录下新建一个config文件，添加如下内容（其中Host和HostName填写git服务器的域名，IdentityFile指定私钥的路径）

``` xml
# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/gitee_id_rsa

#github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/github_id_rsa
```

④用ssh命令分别测试

``` xml
ssh -T git@gitee.com
ssh -T git@github.com
```

参考资料：

- [Git配置多个SSH-Key](https://gitee.com/help/articles/4229#github)
- [一台电脑配置多个ssh key（不同的多个邮箱ssh key，多git账号，智能选择对应的ssh key）](https://blog.csdn.net/yimingsilence/article/details/79980135)
- [管理git生成的多个ssh key](https://www.jianshu.com/p/f7f4142a1556)

## 6. 如何将GitHub已有的项目转移到组织仓库中去

打开仓库，点击 setting，拉到最下面，点击 Transfer，会看到要求输入如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190127214816.png)

在这里分别输入要转移的项目名称，第二行输入组织名。输入完毕之后点击`I understand transfer this repository`。

参考资料：[github如何将已有的项目转移到组织仓库中去](https://blog.csdn.net/u011328417/article/details/74584487)

## 7. 如何在GitHub上添加协议？

①进入你的“代码仓库”，点击"Create new file"，这时 GitHub 的新页面上，有一个空格让你填入文件名称。

②在输入框输入文件名”LICENSE"，这里输入框的右侧会出现包含所有开源协议的列表，选择合适的开源协议，选择你需要的协议；

③点击“Commit new file”，这时你添加的开源协议就在代码仓库的菜单中了。

参考：[如何在github上添加协议](https://www.jianshu.com/p/e4d6e6a05f14)

## 8. 保持码云Gitee和GitHub同步更新？

使用 GitHub 时，国内的用户经常遇到的问题是访问速度太慢，有时候还会出现无法连接的情况（原因你懂的）。如果我们希望体验 Git 飞一般的速度，可以使用国内的 Git 托管服务——[码云](https://gitee.com/)（[gitee.com](https://gitee.com/)）。和 GitHub 相比，码云也提供免费的 Git 仓库。此外，还集成了代码质量检测、项目演示等功能。对于团队协作开发，码云还提供了项目管理、代码托管、文档管理的服务，5 人以下小团队免费。

使用码云和使用 GitHub 类似，在此不赘述了。下面主要讲的是从 GitHub 上 clone 下来的仓库如何与 Gitee 保持同步更新，操作如下：

切换到本地仓库目录，先使用命令：`git remote -v` 查看是否关联了远程仓库。如果显示如下：

``` xml
git remote -v
origin    git@github.com:michaelliao/learngit.git (fetch)
origin    git@github.com:michaelliao/learngit.git (push)
```

说明本地库已经关联了`origin`的远程库，并且，该远程库指向 GitHub。我们可以关联一个远程仓库并指向 Gitee，这样本地库就既关联 GitHub，又关联码云。

PS：使用多个远程库时，我们要注意，Git 给远程库起的默认名称是`origin`，如果有多个远程库，我们需要用不同的名称来标识不同的远程库。

接下来，我们再关联码云远程仓库：

``` xml
git remote add gitee git@gitee.com:liaoxuefeng/learngit.git
```

注意：远程库的名称叫`gitee`，不叫`origin`。

现在，我们用`git remote -v`查看远程库信息，可以看到两个远程库：

``` xml
git remote -v
gitee    git@gitee.com:liaoxuefeng/learngit.git (fetch)
gitee    git@gitee.com:liaoxuefeng/learngit.git (push)
github    git@github.com:michaelliao/learngit.git (fetch)
github    git@github.com:michaelliao/learngit.git (push)
```

如果要推送到 GitHub，使用命令：

``` xml
git push github master
```

如果要推送到码云，使用命令：

``` xml
git push gitee master
```

> 注意：本人用的同一个 ssh-key 的情况下，在提交代码使用简短命令：`git push`时候貌似只提交到了 GitHub 远程仓库；若要提交到 Gitee，则再需`git push gitee master`。

参考：[使用码云 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00150154460073692d151e784de4d718c67ce836f72c7c4000)

## 9. 如何正确接收 GitHub 的消息邮件

参考：[如何正确接收 GitHub 的消息邮件](https://github.com/cssmagic/blog/issues/49) 

## 10. 精准分享关键代码

比如你有一个文件里的某一行代码写得非常酷炫或者关键，想分享一下。可以在 url 后面加上，比如，点击下面这 个 url：<https://github.com/AlloyTeam/AlloyTouch/blob/master/alloy_touch.js#L240>，你便会跳到 `alloy_touch.js` 的第 240 行。如果是想分享多行代码，也很简单：url 后面加上 `#L` 开始行号 `-L` 结束行号，比如，AlloyTouch 的运动缓动和逆向缓动函数如下面代码段所示：<https://github.com/AlloyTeam/AlloyTouch/blob/master/alloy_touch.js#L39-L45>，其实也不用记忆你直接在网址后面操作，GitHub 自动会帮你生成 url。比如你点击 39 行，url 变成了：<https://github.com/AlloyTeam/AlloyTouch/blob/master/alloy_touch.js#L39>，再按住 shift 点击 45 行，url 变成了：<https://github.com/AlloyTeam/AlloyTouch/blob/master/alloy_touch.js#L39-L45>，然后你这个 url 就可以复制分享出去了，点击这个 url 的人自动会跳到 39 行，并且 39-45 行高亮。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190303114615.png)

## 11. 通过提交的msg自动关闭issues

比如有人提交了个 issues <https://github.com/AlloyTeam/AlloyTouch/issues/6>，然后你去主干上改代码，改完之后提交填 msg 的时候，填入：`fix https://github.com/AlloyTeam/AlloyTouch/issues/6`，这个 issues 会自动被关闭。当然不仅仅是 fix 这个关键字。下面这些关键字也可以：

``` markdown
close
closes
closed
fixes
fixed
resolve
resolves
resolved
```

## 12. gitattributes设置项目语言

GitHub 会根据相关文件代码的数量来自动识别你这个项目哪个语言代码项目。这就带来了一个问题，比如 AlloyTouch 最开始被识别成 HTML 项目，因为 HTML 例子比 JS 文件多。怎么办呢？gitattributes 来帮助你搞定。在项目的根目录下添加如下 `.gitattributes` 文件便可，里面的：

``` xml
*.html linguist-language=JavaScript
```

主要意思是把所有 html 文件后缀的代码识别成 js 文件。

## 13. 查看自己项目的访问数据

在自己的项目下，点击 Insights，然后再点击 Traffic，里面有 Referring sites 和 Popular content 的详细数据和排名。如：Referring sites

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190303115918.png)

其中 Referring sites 代表大家都是从什么网站来到你的项目的，Popular content 代表大家经常看你项目的哪些文件。

## 14. trending排行榜

来看看怎么查看某类型语言的每日排行榜。比如 JavaSrcipt 每日排行榜：

- JavaScript：<https://github.com/trending/javascript?since=daily>
- Java：<https://github.com/trending/java?since=daily>
- Python：<https://github.com/trending/python?since=daily>

GitHub 推荐：<https://github.com/explore>

## 15. 使用GitHub release发布应用

(1) 创建release

1. 在 repo 的主页上，点击 release，进入 release 界面 

2. 在 release 界面点选 create a new release 

3. 填写相关信息，上传文件 

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190309121245.png)

   4. publish release 

   5. 通过 GitHub 官方提供的 api 可访问我们的 release 信息：

      ``` xml
      /repos/:owner/:repo/releases/:id
      ```

参考：[github release 功能的使用及问题解决 ](https://blog.csdn.net/Eggy2015/article/details/52138751)



# 二、GitHub项目美化

## 1. GitHub项目徽章的添加和设置

GitHub 项目的 README.md 中可以添加徽章（Badge）对项目进行标记和说明，这些好看的小图标不仅简洁美观，而且还包含了清晰易读的信息。

徽标主要由图片和对应的链接（当然，你可以不填）组成，徽标图片的话一般由左半部分的名称和右半部分的值组成。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190127215817.png)

GitHub 徽标的官方网站是：[shields.io/](https://link.juejin.im/?target=http%3A%2F%2Fshields.io%2F)，我们可以在官网预览绝大部分的徽标样式，然后选择自己喜欢的（当然首先需要适用于自己的目标项目）徽标，添加到自己的项目文档中去。

参考资料：

- [为你的Github README生成漂亮的徽章和进度条](https://shikieiki.github.io/2017/03/01/%E4%B8%BA%E4%BD%A0%E7%9A%84Github%E7%94%9F%E6%88%90%E6%BC%82%E4%BA%AE%E7%9A%84%E5%BE%BD%E7%AB%A0%E5%92%8C%E8%BF%9B%E5%BA%A6%E6%9D%A1/)
- [GitHub 项目徽章的添加和设置](https://juejin.im/entry/5907fa59570c3500582d326c)

## 2. 为GitHub项目添加表情

GitHub 支持的表情，官网查询：<https://www.webfx.com/tools/emoji-cheat-sheet/>

格式，如：`:blush:`，显示为:blush:



# 三、GitHub使用细节

## 1. 本地查看远程分支

git clone 默认会把远程仓库整个给 clone下来，但只会在本地默认创建一个 master 分支，如果远程还有其他的分支，此时用 `git branch -a` 查看所有分支。

## 2. GitHub支持多种协议

GitHub 给出的地址不止一个，除了 `git@github.com:xiaoming/test.git` 这个地址，还可以使用`https://github.com/xiaoming/test.git`  这样的地址。实际上，Git 支持多种协议，默认的 `git://` 使用ssh，但也可以使用 https 等其他协议。

使用 https 除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放 http 端口的公司内部就无法使用 ssh 协议而只能用 https。

## 3. 设置默认被推送的分支

``` xml
git push -u origin test #设置默认被推送的分支
git push	#这个时候我推送的远程分支应该是 origin/test
```

查看`git push`关联的远程分支：`git branch -v`。

## 4. ssh-keygen命令

ssh-keygen 命令用于为“ssh”生成、管理和转换认证密钥，它支持 RSA 和 DSA 两种认证密钥。语法：`ssh-keygen(选项)`

参数：

``` xml
-b：指定密钥长度；
-e：读取openssh的私钥或者公钥文件；
-C：添加注释；
-f：指定用来保存密钥的文件名；
-i：读取未加密的ssh-v2兼容的私钥/公钥文件，然后在标准输出设备上显示openssh兼容的私钥/公钥；
-l：显示公钥文件的指纹数据；
-N：提供一个新密语；
-P：提供（旧）密语；
-q：静默模式；
-t：指定要创建的密钥类型。
```

如：`ssh-keygen -t rsa -C "123456@qq.com"`

## 5. Git忽略提交(.gitignore文件)

在使用 Git 的过程中，我们喜欢有的文件比如日志，临时文件，编译的中间文件等不要提交到代码仓库，这时就要设置相应的忽略规则，来忽略这些文件的提交。简单来说一个场景：在你使用`git add .`的时候，遇到了把你不想提交的文件也添加到了缓存中去的情况，比如项目的本地配置信息，如果你上传到 Git 中去其他人 pull 下来的时候就会和他本地的配置有冲突，所以这样的个性化配置文件我们一般不把它推送到 GIt 服务器中，但是又为了偷懒每次添加缓存的时候都想用`git add .`而不是手动一个一个文件添加，该怎么办呢？很简单，Git 为我们提供了一个`.gitignore`文件只要在这个文件中申明那些文件你不希望添加到 Git 中去，这样当你使用`git add .`的时候这些文件就会被自动忽略掉。

对于经常使用 Git 的朋友来说，`.gitignore`配置一定不会陌生。这种方式通过在项目的某个文件夹下定义`.gitignore`文件，在该文件中定义相应的忽略规则，来管理当前文件夹下的文件的 Git 提交行为。`.gitignore `文件是可以提交到公有仓库中，这就为该项目下的所有开发者都共享一套定义好的忽略规则。在`.gitingore`文件中，遵循相应的语法，在每一行指定一个忽略规则。如：

``` xml
*.log
*.temp
/vendor
```

参考：[Git忽略提交规则 - .gitignore配置运维总结](https://www.cnblogs.com/kevingrace/p/5690241.html)

## 6. 如何在README.md中嵌入一个Gist？

适用于 GitHub 的网页，通过 Jekyll，使用 Markdown 中的脚本标记，然后由 Jekyll 处理。因为 Markdown支持 html，所以可以直接使用< script>标签嵌入 Gist。

只需复制 GitHub 提供的 Gist 的嵌入网址，示例，复制以下内容并粘贴到 Markdown 文件：

```
< script src =“https://gist.github.com/nisrulz/11c0d63428b108f10c83.js”>< / script>
```

这样能看到想要的结果。注：以上使用的 Jekyll 方式，亲测，如果是直接嵌入 README.md 文件是不行的，因为 GitHub为了安全性都不能引入自定义的 JS 和 CSS。

参考：

- [Github: How to embed a gist into README.md? - Stack Overflow](https://stackoverflow.com/questions/11622509/github-how-to-embed-a-gist-into-readme-md)
- [Github：如何在README.md中嵌入一个gist？](https://codeday.me/bug/20171022/88303.html)
- [Custom css file for readme.md in a Github repo - Stack Overflow](https://stackoverflow.com/questions/51956361/custom-css-file-for-readme-md-in-a-github-repo)

## 7. 其他网页上面嵌入个人的GitHub仓库？

是否想在其他网页上面嵌入自己的 GitHub 仓库页面，有个 star 或 fork 按钮，如下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190131161941.png)

可以这样写：

``` html
<iframe src="https://ghbtns.com/github-btn.html?user=strivebo&amp;repo=websites-and-tools&amp;type=watch&amp;count=true&amp;size=large" allowtransparency="true" frameborder="0" scrolling="0" width="156px" height="30px"></iframe>
<iframe src="https://ghbtns.com/github-btn.html?user=strivebo&amp;repo=websites-and-tools&amp;type=fork&amp;count=true&amp;size=large" allowtransparency="true" frameborder="0" scrolling="0" width="156px" height="30px"></iframe>
```

把 user 和 repo 改成你自己的就可以了。注：亲测，GitHub 网站页面暂不支持。

## 8. GitHub快捷方式

- issue 中输入冒号 `:` 添加表情
- 任意界面 `shift + ？`显示快捷键
- issue 中选中文字，R 键快速引用



# 参考及推荐资料

- [waylau/github-help](https://github.com/waylau/github-help)
- [GitHub 秘籍-极客学院Wiki](http://wiki.jikexueyuan.com/project/github-secret/)
- [你必须收藏的Github技巧  - 博客园](http://www.cnblogs.com/iamzhanglei/p/6177961.html)



---

*update：2019-07-21* 