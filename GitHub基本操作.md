# 1 .  创建密钥 #

我们如何让本地git项目与远程的github建立联系呢？之里就用的密钥。通俗点叫口令吧！（天王盖地老，宝塔镇河妖。。）

$ cd ~/. ssh 检查本机的ssh密钥

如果提示：No such file or directory 说明你是第一次使用git。

如果不是第一次使用，请执行下面的操作,清理原有ssh密钥。

    
   	 $ mkdir key_backup
     $ cp id_rsa* key_backup
     $ rm id_rsa*
生成新的密钥：

	ssh-keygen –t rsa –C “cuifromhangzhou@gmai.com” 
 

注意: 此处的邮箱地址，你可以输入自己的邮箱地址。在回车中会提示你输入一个密码，这个密码会在你提交项目时使用，如果为空的话提交项目时则不用输入。这个设置是防止别人往你的项目里提交内容。

 

打开本地C:\Documents and Settings\Administrator\.ssh\id_rsa.pub文件。此文件里面内容为刚才生成人密钥。

登陆github系统。点击右上角的 Account Settings--->SSH Public keys ---> add another public keys

把你本地生成的密钥复制到里面（key文本框中）， 点击 add key 就ok了

在git中运行下面命令：
$ ssh –T git@github.com
如果提示：Hi GitHubOfCui You've successfully authenticated, but GitHub does not provide shell access. 说明你连接成功了。

# 2. 创建本地仓库 #

要使用Git进行版本管理，必须先初始化仓库，Git是使用git init命令初始化的，先建立一个目录并对其初始化

	$ mkdir AliNode
	$ cd AliNode 
	$ git init
	Initialized empty Git repository in ........

初始化成功之后会在当前目录下生成.git目录，这个目录存储着管理当前目录内容所需的仓库数据。在Git中，我们将这个目录的内容称为“附属于该仓库的工作树”。文件的编辑等操作在工作树中进行，然后记录到仓库中，依次管理文件的历史快照。

git status命令用于显示Git仓库的状态

如果只是用Git仓库的工作树创建了文件，那么该文件并不会被记入Git仓库的版本管理对象当中。因此我们用git status查看新文件时，它会显示在UNtracked files里。

要想让文件称为Git仓库的管理对象，就需要用 git add命令将其加入暂存区（Stage或者Index）中。暂存区是提交之前的一个临时区域
	
	git add README.md
	git status
	On branch master
	Initial commit
	Changes to be committed
		(use "git rm --cached <file>..." to unstage)
		
		new file: README.md

git commit命令可以将当前暂存区中的文件实际保存到仓库的历史记录中。通过这些记录，我们就可以在工作树中复原文件。

	git commit -m "First commit"

-m参数后的“First commit”称作提交信息，如果想记录的更加详细，直接执行git commit命令

git log查看以往仓库中提交的日志。包括可以查看什么人在什么时候进行了提交或合并，以及操作前后有怎样的区别。

# 3. 推送至远程仓库 #

Git是分散型版本管理系统，前面所说的都是针对单一本地仓库的操作。下面说的是在GitHub网站上的仓库，我们先在GitHub上创建一个仓库。

在GitHub上创建的仓库路径为“git@github.com:用户名/仓库名.git”。现在我们用git remote add命令将它设置成本地仓库的远程仓库。

	$ git remote add origin git@github.com:GitHubOfCui/AliNode.git
	
按照上诉格式执行git remote add命令之后，Git会自动将git@github.com:GitHubOfCui/AliNode.git远程仓库的名称设置为origin（标识符）。

如果想把当前分支下本地仓库中的内容推送给远程仓库，需要用到git push命令，现在假定我们是在master分支下进行操作

	git push -u origin master
	
想这样执行git push之后，当前分支的内容就被推送到远程仓库origin的master分支。-u参数可以在推送的同时，将origin仓库的master分支设置为本地仓库当前分支的上游。添加了这个参数，将来运行git pull从远程仓库获取内容时，本地仓库的这个分支就可以直接从origin的master分支获取内容，省去了参数的设置。

# 4.从远程仓库获取 #

首先我们换到其他目录下，将GitHub上的仓库clone到本地

	git clone git@github.com:GitHubOfCui/AliNode.git
	
执行git clone命令后我们会默认处于master分支下，同时系统会自动将origin设置成该仓库的标识符。也就是本地仓库的master分支与GitHub端远程仓库（origin）的master分支在内容上完全相同。

git pull --获取最新的远程仓库分支

	$ git pull origin master
	remote: Counting objects:5,done
	...

将远程仓库的master更新到本地，如果之前过仓库名和分支名，可以直接使用git pull跟新当前仓库。







