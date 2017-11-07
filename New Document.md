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
