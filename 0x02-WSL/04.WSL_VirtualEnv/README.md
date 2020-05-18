# Linux 虚拟环境

- 更换源  

    ``` sh
    sudo vim /etc/apt/sources.list
    ```

- 更新：sudo apt-get update

    ```sh
    deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
    ```

- 更换PIP源  
    清华：<https://pypi.tuna.tsinghua.edu.cn/simple>

    阿里云：<http://mirrors.aliyun.com/pypi/simple/>

    中国科技大学 <https://pypi.mirrors.ustc.edu.cn/simple/>

- 更换方法
  - 可以在使用pip的时候加参数-i <https://pypi.tuna.tsinghua.edu.cn/simple>

  - Linux下，修改 ~/.pip/pip.conf (没有就创建一个文件夹及文件。文件夹要加“.”，表示是隐藏文件夹)

      内容如下：

      [global]
      index-url = <https://pypi.tuna.tsinghua.edu.cn/simple>
      [install]
      trusted-host=mirrors.aliyun.com
      windows下，直接在user目录中创建一个pip目录，如：C:\Users\xx\pip，新建文件pip.ini。内容同上。

- 安装python3 pip

    ``` bash
    sudo apt-get install python3.7
    sudo apt-get install python3-pip or sudo easy_install pip
    sudo find / -type f -mount -name pip3
    /usr/bin/pip3
    pip3 list
    ```

- 安装virtualenv

    ``` bash
    # 准备环境
    安装 sudo apt-get install python-pip
    # 查看是否安装虚拟环境
    sudo apt search python-virtualenv
    # 如果没有安装则下载
    sudo apt-get install python-virtualenv

    # 创建虚拟环境
    virtualenv testEnv  (提前 cd 到你指定的目录下)
    # 查看python 版本
    python -V
    # 创建指定的python版本虚拟环境
    virtualenv -p /usr/bin/python3.5  testEnv
    # 激活虚拟环境 或者配置vs code 虚拟环境
    cd testEnv/
    source ./bin/activate or source bin/activate
    # 安装依赖
    pip install flask
    # 退出虚拟环境
    deactivate
    ```

- 安装Virtualenvwrapper  
    sudo apt-get install virtualenvwrapper  

- virtualenvwrapper 命令  
    创建虚拟环境：mkvirtualenv new_env

    激活虚拟环境：workon new_env

    退出虚拟环境：deactivate

    删除虚拟环境: rmvirtualenv new_env

    查看所有虚拟环境：lsvirtualenv

    ``` bash
    # 激活当前环境
    D:\Desktop\todolist>activate todoenv
    # 导出
    (todoenv) D:\Desktop\todolist> pip freeze > requirements.txt
    # 安装的环境文件
    (todoenv) D:\Desktop\todolist> pip install -r requirements.txt
    ```

- pipenv

    ``` bash
    # 列出所有依赖包
    pip list or pip3 list
    # 必须使用 -U 否则 pipenv: command not found
    -i: 指定库的安装源
    -U:升级 原来已经安装的包，不带U不会装新版本，带上U才会更新到最新版本
    pip3 install -U pipenv
    # 安装pipenv
    pip install pipenv
    # 版本
    pipenv --version
    # 查找
    sudo find / -type f -mount -name pipenv
    # 会使用当前系统的Python3创建环境  
    pipenv --three
    # 指定某一Python版本创建环境
    pipenv --python 3.6
    # 激活虚拟环境
    pipenv shell
    # 退出虚拟环境
    exit
    # 显示目录信息
    pipenv --where
    #显示虚拟环境信息
    pipenv --venv
    # 显示Python解释器信息
    pipenv --py
    # 查看目前安装的库及其依赖
    pipenv graph
    # 检查安全漏洞
    pipenv check
    # 卸载全部包并从Pipfile中移除
    pipenv uninstall --all
    #
    pipenv install requests
    #
    pipenv install django==1.11
    #
    pipenv uninstall requests
    # pipenv虚拟环境运行python命令
    pipenv run python your_script.py
    # 安装requirements.txt
    pipenv install -r requirements.txt
    # 像virtualenv一样用命令生成requirements 文件
    pipenv lock -r --dev > requirements.txt
    ```

    设置源  
    Pipfile文件中[source]下面url属性，比如修改成：  
    url = "https://pypi.tuna.tsinghua.edu.cn/simple"  
    or  
    url = "http://mirrors.aliyun.com/pypi/simple/"  
    or default url  
    url = "https://pypi.org/simple"

- Anaconda 命令
- <https://www.cnblogs.com/hafiz/p/9085405.html>
- <https://www.jianshu.com/p/52d848317c17>
- 彻底卸载 <https://www.jianshu.com/p/50032c3cfa28>