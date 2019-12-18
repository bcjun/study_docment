### linux常用命令

| 命令     | 功能                 | 其他参数         |
| -------- | -------------------- | ---------------- |
| ls       | 查看当前路径下的文件 | -h-l-a           |
| clear    | 清屏                 |                  |
| cd       | 换目录               |                  |
| pwd      | 查看当前所在目录     |                  |
| mkdir    | 创建目录             | -p归创建文件     |
| touch    | 创建文件             |                  |
| rm       | 删除文件             | -r 删除文件夹    |
| cp       | 复制文件             | -r 递归拷贝      |
| mv       | 移动文件             |                  |
| tree     | 文件列表树状图       |                  |
| chmod    | 修改文件权限         |                  |
| find     | 查找文件             |                  |
| grep     | 查找文件内容         |                  |
| ln       | 创建链接文件         | -s 创建软链接    |
| tar      | 打包压缩文件         |                  |
| cat      | 文本查看             |                  |
| vi/vim   | vim编辑              |                  |
| gedit    | 查看编辑文本         |                  |
| shutdown | 关机                 |                  |
| reboot   | 重启                 |                  |
| who      | 显示当前登录用户     |                  |
| which    | 查看命令位置         |                  |
| exit     | 退出                 | 需要加 -r 用户名 |
| passwd   | 设置用户密码         |                  |
| sudo     | 超级用户授权执行     |                  |
| history  | 历史命令             |                  |
| su       | 切换用户             |                  |
| useradd  | 创建(添加)用户       |                  |
| userdel  | 删除用户             |                  |

### 常用高级命令

| 命令                                                | 功能                  |
| --------------------------------------------------- | --------------------- |
| ifconfig <网卡名称>  down\|up                       | 开启关闭网卡          |
| ssh 用户名@IP                                       | 远程连接              |
| scp FileName RemoteUserName@RemoteHostIp:RemoteFile | 远程拷贝              |
| ifconfig                                            | 查看IP地址            |
| ping                                                | 远程IP是否可以连接    |
| lsof -i:8000                                        | 查看端口号是否被使用  |
| kill -9 52566(pid)                                  | 死端口号              |
| ps -ef                                              | 查看后台程序          |
| jobs                                                | 查看加了&号的后端程序 |
| netstat –ntpl                                       | 查看当前端口使用情况  |

~~~shell
ps-aux|grep redis 可以查看正在运行的关于redis的进程
#在cmd中ubantu上的文件靠到windows上面
scp -r python@192.168.124.131:/home/python/Desktop /captcha
#在cmd中windows上的文件靠到ubantu上面
scp -r /yuntongxun python@192.168.124.131:/home/python/Desktop
~~~



### Linux主要目录

| 目录名 | 含义                                |
| ------ | ----------------------------------- |
| /bin   | 放可执行二进制文件  如:ls clear cat |
| /boot  | 存放linux启动时的一些文件           |
| /dev   | 存放设备文件                        |
| /etc   | 系统配置文件                        |
| /home  | 系统默认的用户家目录的根目录        |
| /opt   | 给主机额外安装软件所摆放的目录      |
| /lib   | 系统使用的函数库的目录              |
| /proc  | 此目录的数据都在内存中              |
| /root  | 系统管理员root的家目录              |
| /sbin  | 放置系统管理员使用的可执行命令      |
| /usr   | Unix 操作系统软件资源               |
| /srv   | 服务启动之后需要访问的数据目录      |
| /var   | 一般存日志文件                      |

### 压缩打包

+ 打包并压缩

  > ~~~shell
  > tar -czvf NewFile.tar.gz OldFile.tar.gz
  > ~~~

+ 解包并解压

  > ~~~shell
  > tar -zxvf book.tar.gz -C 文件夹名
  > tar -jxvf book.bz2 -C 文件夹名
  > unzip book.zip -d 文件夹名
  > ~~~

### 软件安装与卸载

| 安装方式           | 实现                                   |
| ------------------ | -------------------------------------- |
| 离线安装:          | 直接解压gzip或bzip2压缩文件成deb格式的 |
|                    | sudo dpkg -i xxx.deb                   |
| 在线安装           | sudo apt-get update  #更新源           |
|                    | sudo apt-get install package #安装包   |
| **卸载方式**       | **实现**                               |
|                    | sudo dpkg –r 安装包名                  |
|                    | sudo apt-get remove 安装包名           |
| **Python卸载**     | **实现**                               |
|                    | pip uninstall 包名                     |
| **Python安装**     | **实现**                               |
|                    | pip install 包名                       |
| **python批量安装** |                                        |
|                    | pip freeze                             |
|                    | pip install -r 文件名                  |

### 其他

| 命令                | 解析                                      |
| ------------------- | ----------------------------------------- |
| python@ubuntu:/opt$ |                                           |
| python              | 用户名                                    |
| ubuntu              | 主机名                                    |
| /opt                | 当前所在路径                              |
| $                   | 用户类型  ($:表示普通用  #：表示超级用户) |
| sudo passwd root    | 修改超级用户密码                          |
| su                  | 切换到超级用户                            |

### Vim使用

| 末行模式命令       | 功能            |
| ------------------ | --------------- |
| :w                 | 保存            |
| :wq                | 保存退出        |
| :x                 | 保存退出        |
| :q!                | 强制退出        |
| **vim 的常用命令** | **功能**        |
| yy                 | 复制光标所在行  |
| p                  | 粘贴            |
| dd                 | 删除/剪切当前行 |
| u                  | 撤销            |
| :/搜索的内容       | 搜索指定内容    |
| G                  | 回到最后一行    |
| gg                 | 回到第一行      |
| 数字+G             | 回到指定行      |

### 虚拟环境

**安装Python虚拟环境**

~~~
sudo pip install virtualenv
sudo pip install virtualenvwrapper
~~~

**linux下面使用虚拟环境**

```shell
新建新的虚拟环境
mkvirtualenv django_py3_1.11 -p python3
```

| 方法                  | 功能                           |
| --------------------- | ------------------------------ |
| mkvirtualenv          | 创建虚拟环境                   |
| rmvirtualenv          | 删除虚拟环境                   |
| workon                | 进入虚拟环境、查看所有虚拟环境 |
| deactivate            | 退出虚拟环境                   |
| pip install           | 安装依赖包                     |
| pip uninstall         | 卸载依赖包                     |
| pip list              | 查看已安装的依赖库             |
| pip install -r 文件名 | 批量安装txt里面的包            |

**windows下使用虚拟环境**

> ```
> virtualenv -p C:\Python36\python.exe django_py3_1.11
> ```

| 方法                 | 功能         |
| -------------------- | ------------ |
| activate venv        | 激活虚拟环境 |
| deactivate           | 停止虚拟环境 |
| 直接删除文件就可以了 | 删除虚拟环境 |

**[windows常用命令](https://blog.csdn.net/qq_32451373/article/details/77743869)**

| 命令           | 功能             |
| -------------- | ---------------- |
| cd             | 切换目录         |
| dir            | 显示目录中的内容 |
| ren            | 文件或目录重命名 |
| md             | 创建目录         |
| rd s /q 文件名 | 删除目录         |
| copy           | 拷贝文件         |
| del            | 删除文件         |



