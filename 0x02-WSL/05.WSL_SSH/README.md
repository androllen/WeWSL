---
title: Windows10子系统Ubuntu使用 SSH 登陆
---

- 重新安装

  sudo apt install openssh-server

- 切换到ssh配置文件的位置

  cd /etc/ssh

  ```sh
  # 先把原来的备份
  sudo cp sshd_config sshd_config.bak
  # 先把原来的备份
  sudo cp ssh_config ssh_config.bak
  # 编辑配置文件
  sudo vim sshd_config
  ```

- 修改sshd_config配置

  先查看下本地 Windows10 sshd server是否打开：

  - 没有打开

    ```sh
    # 修改端口
    Port 22
    # 打开本地监听
    ListenAddress 127.0.0.1
    or
    # 打开局域网监听
    ListenAddress 0.0.0.0
    # 修改登陆的方式，允许密码登陆
    PasswordAuthentication yes
    ```

  - 打开

    ```sh
    # 修改端口，原来的22端口已经存在
    Port 2232
    # 打开本地监听
    ListenAddress 127.0.0.1
    or
    # 打开局域网监听
    ListenAddress 0.0.0.0
    # 修改登陆的方式，允许密码登陆
    PasswordAuthentication yes
    ```

- 启动SSH

  sudo service ssh start

  ```sh
  # 会出现找不到host key
  sshd: no hostkeys available -- exiting.
  # 或者
  Could not load host key: /etc/ssh/ssh_host_rsa_key
  Could not load host key: /etc/ssh/ssh_host_ecdsa_key
  Could not load host key: /etc/ssh/ssh_host_ed25519_key
  ```

- 重新生成 host key

  sudo dpkg-reconfigure openssh-server

- 重新启动SSH

  sudo service ssh restart

- [设置用户名高亮](https://askubuntu.com/questions/123268/changing-colors-for-user-host-directory-information-in-terminal-command-prompt)

  ```sh
  cd $HOME
  cp .bashrc .bashrc.bak
  sudo vim .bashrc
  ```

  找到带有 `#force_color_prompt=yes` 去掉 `#`  
  找到下面含有 `if [ "$color_prompt" = yes ]; then`

  ``` sh
  # PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
  # 替换上面 PS1
  PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u\[\033[01;36m\]@\[\033[01;32m\]\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
  # 更新配置
  source .bashrc
  ```

  ```sh
  Black       0;30     Dark Gray     1;30
  Blue        0;34     Light Blue    1;34
  Green       0;32     Light Green   1;32
  Cyan        0;36     Light Cyan    1;36
  Red         0;31     Light Red     1;31
  Purple      0;35     Light Purple  1;35
  Brown       0;33     Yellow        1;33
  Light Gray  0;37     White         1;37  
  ```

- 配置SSH自动启动

  sudo systemctl enable ssh

- check ssh auto status

  sudo systemctl is-enabled ssh

- 设置服务权限免密码登录
  
  ```sh
  # 添加 sudoers 文件的写权限
  sudo chmod u+w /etc/sudoers
  sudo vim /etc/sudoers
  # 在最后一行输入 ( androllen 安装 wsl 时你的用户名)
  androllen ALL=(ALL) NOPASSWD: /usr/sbin/service
  or
  # C:\Users\%USERNAME%\AppData\Local\Packages
  androllen ALL=(ALL) NOPASSWD: ALL
  # 撤销 sudoers 文件写权限
  sudo chmod u-w /etc/sudoers
  ```

  ```sh
  bash.exe -c 'sudo service ssh start'
  or
  wsl.exe -u androllen sudo service ssh start
  ```

- 保存 [startWSL.vbs][wslvbs_id]

  ```sh
  # cd %AppData%\Microsoft\Windows\Start Menu\Programs\Startup or Win + R -> shell:startup
  set ws=wscript.createobject("wscript.shell")
  cmd = "C:\Windows\System32\bash.exe -c 'sudo /usr/sbin/service ssh start'"
  ws.run cmd,0
  ```

- 打开任务计划程序

  - Win + R
  - taskschd.msc
  - ![home](Assets/20190514132518.png)
  - ![input](Assets/20190514132721.png)
  - ![task](Assets/20190514132845.png)
  - ![start](Assets/20190514133108.png)
  - ![vbs](Assets/20190514133140.png)
  - ![complete](Assets/20190514133202.png)
  - [文件下载][taskvbs_id]
  - 或者选择导入
  - 重启 Windows

[开启ssh服务](https://www.cnblogs.com/seekwind/p/10256262.html)  

[wslvbs_id]: Assets/startWSL.vbs
[taskvbs_id]: Assets/AutoService.xml
