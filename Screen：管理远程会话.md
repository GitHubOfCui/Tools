&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;你是不是经常需要 SSH 或者 telent 远程登录到 Linux 服务器？你是不是经常为一些长时间运行的任务而头疼，比如系统备份、ftp 传输等等。通常情况下我们都是为每一个这样的任务开一个远程终端窗口，因为他们执行的时间太长了。必须等待它执行完毕，在此期间可不能关掉窗口或者断开连接，否则这个任务就会被杀掉，一切半途而废了。

元凶：SIGHUP信号
==
让我们看看为什么关掉窗口/断开连接会使得正在运行的程序死掉。
在linux中，有这样几个概念：
1. 进程组：一个或多个进程的集合，每一个进程组有唯一一个进程组ID，即进程组长进程的ID。
2. 会话期：一个或多个进程组的集合，有唯一一个会话期首进程（session leader）。会话期ID为首进程的ID。
3. 会话期可以有一个单独的控制终端（controlling terminal），与控制终端连接的会话期叫做控制进程，当前与终端交互的进程称为前台进程组，其余进程组称为后台进程组。

根据POSIX.1定义：
1. 挂断信号（SIGHUP）默认的动作是终止程序。
2. 当前终端接口检测到网络连接断开，将挂断信号发送给控制进程（会话期首进程）
3. 如果会话期首进程终止，则该信号发送到该会话期前台进程组
4. 一个进程退出导致一个孤儿进程组中产生时，如果任意一个孤儿进程组处于STOP状态，发送SIGHUP和SIGCONT信号到该进程组中所有进程。

因此当网络连接断开或窗口关闭后，控制进程收到SIGHUP信号退出，会导致该会话期内其他进程退出。

开始使用screen命令
==
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;简单来说，Screen是一个可以在多个进程之间多路复用一个物理终端的窗口管理器。Screen中有会话的概念，用户可以在一个screen会话中创建多个screen窗口，在每一个screen窗口中就像操作一个真实的telnet/SSH连接窗口那样。在screen中创建一个新的窗口有这样几种方式：
直接在命令行键入screen命令
__
    [root@ibnode8 mxnet]# screen

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Screen将创建一个执行shell的全屏窗口。你可以执行任意shell程序，就像在ssh窗口中那样。在该窗口中键入exit退出该窗口，如果这是该screen会话的唯一窗口，该screen会话退出，否则screen自动切换到前一个窗口。

2. 给screen会话命名
__
    [root@ibnode8 mxnet]# screen -S screen_name

给screen会话命名使管理更容易。

3. Screen命令后跟你要执行的程序
__
    [root@ibnode8 mxnet]# screen vi test.c

Screen创建一个执行vi test.c的单窗口会话，退出vi将退出该窗口/对话。

4. 查看当前screen会话列表
__
    [root@ibnode8 mxnet]# screen -ls
    There is a screen on:
	    6725.pts-0.ibnode8	(Detached)
    1 Socket in /var/run/screen/S-root.

5. screen会话的断开与重连
__
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当我们登入一个screen会话的之后如果要退出可使用CRTL+D，这个方法会强制终止当前会话。如果我们之后还想继续使用当前对话，使用CRTL+A+D命令会让当前screen会话处于detached状态，下次如果要使用可以用screen -r screen_name/screen_pid进行重连。

6. 更多screen功能
__
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Screen提供了丰富强大的定制功能。你可以在Screen的默认两级配置文件/etc/screenrc和$HOME/.screenrc中指定更多，例如设定screen选项，定制绑定键，设定screen会话自启动窗口，启用多用户模式，定制用户访问权限控制等等。如果你愿意的话，也可以自己指定screen配置文件。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;以多用户功能为例，screen默认是以单用户模式运行的，你需要在配置文件中指定multiuser on 来打开多用户模式，通过acl*（acladd,acldel,aclchg...）命令，你可以灵活配置其他用户访问你的screen会话。更多配置文件内容请参考screen的man页。
