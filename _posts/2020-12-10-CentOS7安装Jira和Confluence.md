---
title: CentOS7安装Jira和Confluence以及插件破解教程
tags: CentOS Jira Confluence Atlassian 破解 教程
key: CentOS7instalJiraAndConfluence
---

# CentOS7安装Jira和Confluence以及插件破解教程

## CentOS7安装

### CentOS7安装启动盘

1. 下载CentOS7系统镜像：[CentOS7](http://mirrors.aliyun.com/centos/7/isos/x86_64/CentOS-7-x86_64-Everything-2009.iso)
2. 准备一个16G以上U盘，格式化。
3. 查看U盘盘符，并记录

    ```bash
    diskutil list
    ```

4. 取消挂载U盘

    ```bash
    diskutil umountDisk /dev/sdb
    ```

5. 格式化u盘

    ```bash
    sudo mkfs.vfat /dev/sdb -I
    ```

6. 制作启动盘

    ```bash
    sudo dd if=$镜像路径.iso$ of=/dev/sdb status=progress
    ```

### CentOS7安装系统

将U盘启动盘插入需要安装的服务器电脑，设置电脑Bios从u盘启动。

启动电脑后选第一项进入系统安装CentOS。
选择语言

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/20201210155047.png)

设置时区

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/20201210155127.png)

安装时选择需要安装的软件：

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/20201210154804.png)

选择Server with Gui，然后在右边选择带Java Platform

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/20201210154959.png)

选择安装位置

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/20201210155220.png)

设置主机名和网卡信息

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/20201210155317.png)

开始安装

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/20201210155352.png)

设置root密码

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/20201210155422.png)

设置User Creation创建管理员账号

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/20201210155507.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/20201210155548.png)

安装完成后重启进入CentOS7系统

### CentOS7更换国内源

1. 备份(针对所有CentOS可用，备份文件在当前路径下)

`mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup`

2. 下载新的CentOS-Base.repo 到/etc/yum.repos.d/

```bash
# 阿里云源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
# 或
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
# 网易云源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo
```

## Mysql数据安装与配置

### Mysql数据库的安装

1. 下载[mysql community server 5.7版本](https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.31-1.el7.x86_64.rpm-bundle.tar)
(其他版本可以在官网下载[https://downloads.mysql.com/archives/community/](https://downloads.mysql.com/archives/community/))
2. 安装之前先卸载系统自带的MariaDB

    ```bash
    [root@jira soft]# rpm -qa | grep mariadb
    mariadb-libs-5.5.60-1.el7_5.x86_64
    [root@jira soft]# rpm -e mariadb-libs-5.5.60-1.el7_5.x86_64 --nodeps
    ```

3. 检查是否已经存在mysql，若存在卸载，避免安装时产生一些错误！

    ```bash
    rpm -qa | grep -i mysql
    ```

    若存在mysql，则删除现有mysql

    ```bash
    rpm -e mysql –nodeps
    ```

    查看mysql配置信息，删除对应的配置文件等信息！

    ```bash
    which mysql
    ```

    删除mysql用户

    ```bash
    userdel mysql
    ```

4. 解压缩下载的mysql安装包

    ```bash
    tar -xvf mysql-5.7.31-1.el7.x86_64.rpm-bundle.tar
    ```

    解压后得到如下文件：
    
![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%884.22.44.png)

5. 安装mysql

    ```bash
    rpm -ivh mysql-community-common-5.7.31-1.el7.x86_64.rpm
    rpm -ivh mysql-community-libs-5.7.31-1.el7.x86_64.rpm
    rpm -ivh mysql-community-libs-compat-5.7.31-1.el7.x86_64.rpm
    rpm -ivh mysql-community-devel-5.7.31-1.el7.x86_64.rpm
    rpm -ivh mysql-community-server-5.7.31-1.el7.x86_64.rpm
    rpm -ivh mysql-community-client-5.7.31-1.el7.x86_64.rpm
    ```

### 配置数据库

1. 修改数据库的配置

    ```bash
    [root@jira ~]# vim /etc/my.cnf
    [mysqld]
    ...
    default-storage-engine=INNODB
    character_set_server=utf8mb4
    innodb_default_row_format=DYNAMIC
    innodb_file_format=Barracuda
    innodb_log_file_size=2G
    sql_mode=ONLY_FULL_GROUP_BY,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
    
    #这里修改后，后面就无需再次修改了
    ```

2. 启动数据库并设置开机自启动

    ```bash
    systemctl start mysqld
    systemctl enable mysqld
    ```

3. 查看默认密码登陆数据库

    ```bash
    cat /var/log/mysqld.log |grep password
    ```

4. 登陆数据库,并输入默认密码

    ```bash
    mysql -uroot -p
    ```

5. 重置root密码

    ```bash
    mysql> set global validate_password_policy=0;
    mysql> set global validate_password_number_count=0; 
    mysql> set global validate_password_special_char_count=0; 
    mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'PASSWORD';
    mysql> CREATE USER 'root'@'%' IDENTIFIED BY 'PASSWORD';
    mysql> grant all privileges on *.* to root@'%';
    ```

6. 创建Jira用户和库，并配置权限

    ```bash
    mysql> CREATE DATABASE jira CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
    mysql> GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,REFERENCES,ALTER,INDEX on jira.* TO 'jira'@'%' IDENTIFIED BY 'PASSWORD';
    mysql> flush privileges;
    ```

7. 创建Confluence用户和库，并配置权限

    ```bash
    mysql> CREATE DATABASE confluence CHARACTER SET utf8 COLLATE utf8_bin; 
    mysql> CREATE USER 'confluence'@'%' IDENTIFIED BY 'PASSWORD';
    mysql> GRANT ALL PRIVILEGES ON confluence.* TO 'confluence '@'localhost' IDENTIFIED BY 'PASSWORD';
    mysql> GRANT ALL PRIVILEGES ON confluence.* TO 'confluence'@'%' IDENTIFIED BY 'PASSWORD';
    ```

## Jira安装与破解

### 下载Jira
    打开[Jira网页](https://www.atlassian.com/zh/software/jira/pricing?tab=self-managed)，下载[自托管的服务器软件](https://product-downloads.atlassian.com/software/jira/downloads/atlassian-jira-software-8.13.2-x64.bin)。

### 赋予权限并安装

    ```bash
    sudo chmod +x atlassian-jira-software-8.13.2-x64.bin
    sudo ./atlassian-jira-software-8.13.2-x64.bin
    ```

- 安装到最后选择`Start Jira Software X.X.X now?`时选择 `No[n]`。

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/image-110.png)

### 开放8080端口并对Jira目录进行授权

    ```bash
    sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
    sudo firewall-cmd --reload

    sudo chown -R jira:jira /var/atlassian/
    sudo chown -R jira:jira /opt/atlassian/
    ```

### 下载破解文件

- 下载[atlassian-agent](https://gitee.com/pengzhile/atlassian-agent/attach_files/283101/download/atlassian-agent-v1.2.3.tar.gz)并解压。此项目仓库地址为[https://gitee.com/pengzhile/atlassian-agent](https://gitee.com/pengzhile/atlassian-agent)

- 复制atlassian-agent.jar文件到/opt/atlassian/目录下

    `sudo cp ./atlassian-agent.jar /opt/atlassian/`

- 下载[一键配置脚本](https://raw.githubusercontent.com/alues/atlassian_install_script/master/Install.sh)至自定义目录，以sudo权限启动，并选`1) install`。

    `sudo sh ./install.sh`

### 下载用于Jira的mysql驱动文件

    mysql驱动文件下载地址：[点击下载](https://dev.mysql.com/downloads/connector/j/5.1.html)
    [mysql-connector-java](https://cdn.mysql.com//Downloads/Connector-J/mysql-connector-java-5.1.49.tar.gz)

    解压下载的压缩包后

    ```bash
    sudo cp mysql-connector-java-5.1.49/*.jar /opt/atlassian/jira/atlassian-jira/WEB-INF/lib
    ```

### 启动Jira

    ```bash
    sudo /opt/atlassian/jira/bin/start-jira.sh -fg
    ```

- 检查启动进程中是否包含`-javaagent`参数

    ```bash
    ps -aux|grep java
    ```

- 检查日志是否含有下图输出的日志信息

    ```bash
    sudo cat /opt/atlassian/jira/logs/catalina.out | grep "agent working"
    ```

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%885.52.51.png)

- 如果以上都在日志中有,且日志中没有ERROR级别的，则可以浏览器访问JIRA进行接下来的配置

### 配置Jira

- 浏览器访问 [127.0.0.1:8080](127.0.0.1:8080)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%885.55.31.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%885.56.48.png)

- 这里需要等待程序进行数据初始化，需要一段时间

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%885.58.29.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%886.01.43.png)

### 获取KEY

- 获得上面的服务器ID后，下面通过以下命令进行获取到key

```bash
java -jar /opt/atlassian/atlassian-agent.jar -m email@xxx -n USERNAME -p jc -o http://IP:8080 -s BD77-K35P-74WP-HANA
```

参数说明：

-m：指定邮箱

-n：授权给的用户

-p：需要获取key的程序, jira为Jira Software;jc为jira Core的缩写

-o：jira的url地址

-s：服务器ID

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%886.06.57.png)

### 设置管理员和邮件通知

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%887.44.41.png)

### 最后配置

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%886.09.24.png)

 ![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%887.46.51.png)


### 验证状态

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%887.48.43.png)

开始破解Jira Software

```bash
java -jar /opt/atlassian/atlassian-agent.jar -m email@xxxx -n USERNAME -p jira -o http://IP:8080 -s BD77-K35P-74WP-HANA
```

这里服务器ID不变，只要将`-p`后的参数改成jira即可。

更新Jira Software的许可证

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%887.52.35.png)

## Confluence安装与破解

### 下载Confluence
    
打开[Confluence网页](https://www.atlassian.com/zh/software/confluence/pricing?tab=self-managed)，下载[自托管的服务器软件](https://product-downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-7.4.6-x64.bin)。

### 赋予权限并安装

    ```bash
    sudo chmod +x atlassian-confluence-7.4.6-x64.bin
    sudo ./atlassian-confluence-7.4.6-x64.bin
    ```

- 安装到最后选择`Start Confluence now?`时选择`No[n]`。

    ```bash
    [sunny@localhost ~]$ sudo ./atlassian-confluence-7.4.6-x64.bin
    Unpacking JRE ...
    Starting Installer ...

    This will install Confluence 7.4.6 on your computer.
    OK [o, Enter], Cancel [c]
    o
    Click Next to continue, or Cancel to exit Setup.

    Choose the appropriate installation or upgrade option.
    Please choose one of the following:
    Express Install (uses default settings) [1],
    Custom Install (recommended for advanced users) [2, Enter],
    Upgrade an existing Confluence installation [3]
    1
    The following default options cannot be used. You will be asked to provide alternatives for these options.
    * Home Directory (Existing home directory)


    Default location for Confluence data
    [/var/atlassian/application-data/confluence]


    See where Confluence will be installed and the settings that will be used.
    Installation Directory: /opt/atlassian/confluence
    Home Directory: /var/atlassian/application-data/confluence
    HTTP Port: 8090
    RMI Port: 8000
    Install as service: Yes
    Install [i, Enter], Exit [e]
    i

    Extracting files ...


    Please wait a few moments while we configure Confluence.

    Installation of Confluence 7.4.6 is complete
    Start Confluence now?
    Yes [y, Enter], No [n]
    n

    Installation of Confluence 7.4.6 is complete
    Finishing installation ...
    ```

### 开放8090端口

    ```bash
    sudo firewall-cmd --permanent --zone=public --add-port=8090/tcp
    sudo firewall-cmd --reload

    sudo chown -R confluence:confluence /var/atlassian/
    sudo chown -R confluence:confluence /opt/atlassian/
    ```

### 配置破解文件

- 若安装Jira时已配置过`atlassian-agent.jar`，则无需再次下载。若为安装此软件，则参照安装Jira教程。

- 下载[一键配置脚本](https://raw.githubusercontent.com/alues/atlassian_install_script/master/Install.sh)至自定义目录，以`sudo`权限启动，并选`1) install`。

    ```bash
    sudo sh ./install.sh
    ```

### 下载用于Confluence的mysql驱动文件

    (这个驱动文件与安装Jira时的mysql驱动文件相同)
    mysql驱动文件下载地址：[点击下载](https://dev.mysql.com/downloads/connector/j/5.1.html) 
    [mysql-connector-java](https://cdn.mysql.com//Downloads/Connector-J/mysql-connector-java-5.1.49.tar.gz)

    解压下载的压缩包后：

    ```bash
    sudo cp mysql-connector-java-5.1.49/*.jar /opt/atlassian/confluence/confluence/WEB-INF/lib
    ```


### 启动Confluence

    ```bash
    sudo /opt/atlassian/confluence/bin/start-confluence.sh
    sudo /opt/atlassian/confluence/bin/start-confluence.sh -fg
    ```

- 检查启动进程中是否包含`-javaagent`参数

    ```bash
    ps -aux|grep java
    ```

- 检查日志是否含有下列输出的日志信息

    ```bash
    sudo cat /opt/atlassian/confluence/logs/catalina.out | grep "agent working"
    ```

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%889.20.43.png)

- 如果以上都在日志中有,且日志中没有ERROR级别的，则可以浏览器访问Confluence进行接下来的配置

### 配置Confluence

- 浏览器访问[127.0.0.1:8090](127.0.0.1:8090)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%889.25.17.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%889.26.59.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%889.27.37.png)

### 获取KEY

- 获得上面的服务器ID后，下面通过以下命令进行获取到key

    ```bash
    java -jar /opt/atlassian/atlassian-agent.jar -m mail@xxxx -n USERNAME -p  conf -o http://IP:8090 -s BYED-3L7G-YKRG-VKK3
    ```

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%889.32.43.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%889.34.32.png)

### 设置数据库

数据库类型选择`MySQL`,安装类型选择`通过连接字符串`。

在`数据库URL`中填写：`jdbc:mysql://localhost/confluence?sessionVariables=tx_isolation='READ-COMMITTED'`

下面填写配置数据库时设置的用户名和密码即可测试连接。

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%889.39.02.png)

点击下一步，等待服务器设置好数据库。

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%889.44.29.png)

选择空白站点。

配置用户管理中选择`在Confluence中管理用户和组`。

设置用户名和密码，最后就设置成功。


### Tomcat配置不正确解决

```bash
sudo nano /opt/atlassian/confluence/conf/server.xml
```

更改里面的`proxyName`改为IP地址,`proxyPort`改为`8090`，然后重启confluence

## Confluence与Jira整合统一用户管理

### 配置Jira

在Jira页面找到设置里的`用户管理`

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8810.53.48.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8810.54.54.png)

### 配置Confluence

- 找到设置中的安全配置

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8810.56.32.png)

- 添加Jira目录类型

在用户目录中添加`Atlassian JIRA`目录类型

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8810.58.19.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8810.59.10.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8810.59.37.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8811.00.02.png)

点击下面的`测试设置`

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8811.01.19.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8811.01.52.png)

### 测试同步用户信息

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8811.02.44.png)

- 输入一个Jira用户，进行测试

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8811.03.35.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8811.04.21.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8811.05.12.png)


## Confluence配置文章关联Jira任务

### 配置Jira应用程序链接

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8811.07.32.png)

![](https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/%E6%88%AA%E5%B1%8F2020-12-10%20%E4%B8%8B%E5%8D%8811.08.05.png)
