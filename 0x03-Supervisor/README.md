Supervisor
* 介绍  
    在linux或者unix操作系统中，守护进程（Daemon）是一种运行在后台的特殊进程，它独立于控制终端并且周期性的执行某种任务或等待处理某些发生的事件。  
    > Supervisord是supervisor的服务端程序。运行 Supervisor 时会启动一个进程 supervisord，它负责启动所管理的进程，并将所管理的进程作为自己的子进程来启动，而且可以在所管理的进程出现崩溃时自动重启。
    > Supervisorctl 是命令行管理工具，可以用来执行 stop、start、restart 等命令，来对这些子进程进行管理。
* 应用场景   
    应用在dotnet & python 等

* 环境：Ubuntu 18 

* 安装: sudo apt-get install supervisor（建议安装）

* 配置文件详解（supervisor config file）
    ```
    # 服务
    [unix_http_server]  
    # 这个主要是定义supervisord这个服务端进程的一些参数的 这个必须设置，不设置，supervisor就不用干活了
    [supervisord]          
    # 这个主要是针对supervisorctl的一些配置
    [supervisorctl]   
    # 这个东西挺有用的，当我们要管理的进程很多的时候，写在一个文件里面就有点大了。我们可以把配置信息写到多个文件中，然后include过来
    [include]  
    ```

* 配置自定义文件
    ```
    cd /etc/supervisor/conf.d
    sudo vi test.conf
    ```

    ```
    [program:we-todo]
    ; the program (relative uses PATH, can take args)
    command=/home/androllen/todoenv/bin/python /home/androllen/todolist/run.py
    ; directory to cwd to before exec (def no cwd)
    directory=/home/androllen/todolist              
    ; start at supervisord start (default: true)								
    autostart=true                                  			
    ; whether/when to restart (default: unexpected)					
    autorestart=true                                		
    ; number of secs prog must stay running (def. 1)						
    startsecs=1                                     							
    ; max # of serial start failures (default 3)	
    startretries=3                                  			                   				
    ; stdout log path, NONE for none; default AUTO				
    stdout_logfile=/var/log/supervisor/supervisor_out.log			
    ; number of bytes in 'capturemode' (default 0)				
    stdout_capture_maxbytes=1MB                     				
    ; stderr log path, NONE for none; default AUTO				
    stderr_logfile=/var/log/supervisor/supervisor_err.log			
    ; number of bytes in 'capturemode' (default 0)				
    stderr_capture_maxbytes=1MB                     								
    ```

* 修改supervisord.conf Ui查看运行的自定义配置服务 
    ```
    [inet_http_server]
    port=127.0.0.1:9001
    username=user
    password=123
    访问这个地址 http://127.0.0.1:9001就能跳转到supervisord 的服务管理界面
    ```


* 命令管理
    ```
    # which supervisord
    # 配置 supervisord 开机启动
    sudo systemctl enable supervisor
    # 查看 supervisord 开机启动状态
    sudo systemctl is-enabled supervisor
    # 查看服务是否开启 
    sudo service supervisor status
    # 开启 
    sudo service supervisor start
    # 运行supervisord （同开启是一样的效果嘛？）
    sudo supervisord -c /etc/supervisor/supervisord.conf  or /usr/bin/supervisord -c /etc/supervisor/supervisord.conf
    # 停止 
    sudo service supervisor stop
    # 帮助
    sudo supervisord -h
    # 查看是否生效
    ps -ef | grep supervisord
    root      1266     1  0 May09 ?        00:00:00 /usr/bin/python /usr/bin/supervisord -c supervisord.conf
    androll+  1775  1360  0 12:19 tty1     00:00:00 grep --color=auto supervisord
    ```

* Supervisorctl 命令介绍
    ```
    #关闭所有任务 
    sudo supervisorctl shutdown 
    #查看所有任务状态
    sudo supervisorctl status 
    # 停止某一个进程，program_name 为 [program:x] 里的 x
    sudo supervisorctl stop program_name
    # 启动某个进程
    sudo supervisorctl start program_name
    # 重启某个进程
    sudo supervisorctl restart program_name
    # 结束所有属于名为 groupworker 这个分组的进程 (start，restart 同理)
    sudo supervisorctl stop groupworker:
    # 结束 groupworker:name1 这个进程 (start，restart 同理)
    sudo supervisorctl stop groupworker:name1
    # 停止全部进程，注：start、restart、stop 都不会载入最新的配置文件 先关闭supervisor启动脚本，之后再关闭supervisord服务
    sudo supervisorctl stop all
    # 载入最新的配置文件，停止原有进程并按新的配置启动、管理所有进程
    sudo supervisorctl reload
    # 根据最新的配置文件，启动新配置或有改动的进程，配置没有改动的进程不会受影响而重启
    sudo supervisorctl update
    ```


* 问题  
    1.Error: Another program is already listening on a port that one of our HTTP servers is configured to use.   
    Shut this program down first before starting supervisord.  
    For help, use /usr/bin/supervisord -h  
    是因为有一个使用supervisor配置的应用程序正在运行，需要执行命令终止它或重新创建一个ProjectName.conf文件再执行第一条命令。  
    sudo supervisorctl shutdown  
    2.如果运行supervisorctl出现以下错误   
    error: <class 'socket.error'>, [Errno 111] Connection refused: file: /usr/lib64/python2.6/socket.py line: 567  
    可能是由于supervisord进程停止了，建议重新运行  
    sudo supervisord -c /etc/supervisor/supervisord.conf  


* 相关地址
    http://supervisord.org/  
    http://supervisord.org/configuration.html#program-x-section-settings  
    https://blog.csdn.net/u013421629/article/details/79174313  
