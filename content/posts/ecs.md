---
title: "阿里云ECS(Elastic Compute Service)"
date: "2022-04-25"
tags: ["瞎折腾"]
summary: "天下没有免费的午餐"
---

　　今天申请免费试用了阿里云的ECS，两个星期，后续申请两个月免费使用的话，要发啥使用体验，再说吧，到时候把这篇文章发过去，腾讯的理念一直可以，新用户50一年，没啥花里胡哨的引流，到时候喜欢就直接买腾讯云了。

# SSH连接

1. 在控制台里创建密匙对，并绑定密匙对

2. 开机重启（重启后服务器的`~/.ssh/authorized_keys`里就写入了公匙）

   公匙好像是重启后自动放在了被访问的主机里，然后下载的私匙要自己放在客户机的`~/.ssh/[name].pem`里。阿里云的`.pem`后缀没搞太懂，不过好像和 gitee 里 git 的密匙差不多。

3. 修改私匙文件属性`chmod 400 [.pem私钥文件在本地机上的存储路径]`

4. 然后 `vim ~/.ssh/config`文件，里面这么配置：（可以多台）

   ```bash
   Host [name]
     HostName [公网IP]
   # 私有IP应该是阿里云内部机子互相访问用的？
     Port [端口]
     User [root或username]
     IdentityFile ~/.ssh/[name].pem
   ```

5. 哦对，我最开始就是忘记安 `openssh`了，所以这里出错了，不安 `openssh`可以用密码登录，但是怪麻烦的，安装完之后，`service sshd restart`就好了不晓得 win10 安装好安不，应该安完设置好 PATH 后，也能用 PowerShell 执行这一句吧。

6. `ssh [config文件里的ECS别名]`就能登上啦，比`ssh root@[公网IP]`再输密码，要好用

win 下还可以在 MobaXterm 里输命令`ssh -i [.pem私钥文件在本地机上的存储路径] root@[公网IP地址]`来配置啊，妙！

安卓端我用的 ConnectBot 和 Termux 都不错，Termux 都能安装 clang、python3了，还用啥服务器？因为文件同步不好使啊。

# 新建用户

```bash
sudo useradd -m -s /bin/bash username
sudo passwd username
```

# 传输

1. 从服务器下载文件`scp username@servername:path/filename local_dir`
2. 从服务器下载目录`scp -r username@servername:remote_dir local_dir`
3. 上传文件到服务器`scp /path/filename username@servername:path`
4. 上传目录到服务器`scp -r local_dir username@servername:remote_dir`
