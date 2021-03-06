**文档目录**

<!-- 目录开始 -->
- [mysqltools简介](#mysqltools简介)
    - [mysqltools特性](#mysqltools特性)
        - 1 mysql、master-slaves、innodb-cluster、mysql-group-replication等mysql相关环境的自动化安装与配置
        - 2 mysql高可用、读写分离、负载均衡 集群的自动化安装与配置
        - 3 mysql全备、增备、验证、还原 整个生命周期的管理
        - 4 mysql监控环境的自动化安装、配置
        - 5 mysql巡检
        - 6 mysql优化
        - 7 私人定制/商务合作/学习交流/技术支持
    - [mysqltools所遵循的标准](#mysqltools所遵循的标准)
        - [标准化的意义](#标准化的意义)
        - [mysqltools中mysql相关的标准](#mysqltools中mysql相关的标准)
    - [安装与配置mysqltools](#安装与配置mysqltools)
        - [安装python](#安装python)
            - [安装python第一步](#安装python第一步)
            - [安装python第二步](#安装python第二步)
            - [自动化安装python](#自动化安装python)
        - [安装ansible](#安装ansible)
            - [安装ansibe第一步解决依赖问题](#安装ansibe第一步解决依赖问题)
                - [安装pycparser](#安装pycparser)
                - [安装six](#安装six)
                - [安装asn1crypto](#安装asn1crypto)
                - [安装idna](#安装idna)
                - [安装cryptography](#安装cryptography)
                - [安装pyasn1](#安装pyasn1)
                - [安装pPyNaCl](#安装pPyNaCl)
                - [安装bcrypt](#安装bcrypt)
                - [安装paramiko](#安装paramiko)
                - [安装PyYAML](#安装PyYAML)
                - [安装MarkupSafe](#安装MarkupSafe)
                - [安装Jinja2](#安装Jinja2)
            - [安装ansibe第二步安装ansible](#安装ansibe第二步安装ansible)
            - [自动化安装ansible](#自动化安装ansible)
        - [其它依赖软件的安装](#mtls_install_self_others)
- [mysqltools快速入门](#mysqltools快速入门)
    - [配置mysqltools](#配置mysqltools)
    - [使用mysqltools](#使用mysqltools)
- [mysqltools功能列表](#mysqltools功能列表)
    - [mysql安装](#mysql安装)
        - [单机实例mysql的安装](#单机实例mysql的安装)
        - [master-slaves复制环境的安装](#mtls_master_slaves_install)
        - [mysql-group-replication环境的安装](#mtls_group_replication)
        - [innodb-cluster环境的安装](#mtls_innodb_cluster)
        - [mysql-cluster环境的安装](#mtls_mysql_cluster)
    - [被控主机上的python安装](#被控主机上的python安装)
    - [Master High Availability(mha)环境的安装](#mtls_mha)
        - [manger 节点的安装](#mtls_mha_manager)
        - [node   节点的安装](#mtls_mha_node)
    - [mysql 中间件(读写分离，负载均衡，数据分片)](#mtls_mysql_proxy)
        - [dble](#爱可生分布式中间件)
        - [mycat](#mtls_mysql_proxy_mycat)
        - [atlas](#mtls_mysql_proxy_atlas)
    - [mysql备份生命周期管理](#mtls_mysql_backups)
        - [基于MySQL Enterprise Backup(meb)备份周期的管理](#mtls_meb_backup)
        - [基于percona-xtrabackup(xtrabackup)备份周期的管理](#mtls_xbk_backup)
    - [mysql监控环境的安装](#mysql监控环境的安装)
        - [安装zabbix自用的后台mysql数据库](#安装zabbix自用的后台mysql数据库)
        - [httpd的安装](#httpd的安装)
        - [php的安装](#php的安装)
        - [zabbix-server的安装](#zabbix-server的安装)
        - [zabbix-agent的安装](#zabbix-agent的安装)
        - [zabbix 自动化监控mysql的配置](#mtls_zabbix_config)
    - [mysql深度巡检](#mtls_mysql_inspection)
    - [mysql 优化](#mtls_mysql_tuning)
        - [mysql 参数优化](#mtls_mysql_tuning_parta)
        - [sql 语句优化](#mtls_mysql_tuning_partb)
- [私人定制/商务合作/学习交流](#mtls_contact)

<!-- 目录结束 -->

<!-- 正文开始 -->
# mysqltools简介
mysqltools 是一个用于快速构建或管理 大规模，高质量，全自动化，mysql分布式集群环境的工具；mysqltools的目标是
能在一天之内交付数以千记的高质量结点组成的环境。

## 标准化的意义
总的来说标准化是自动化的基础，没有标准化就谈不上自动化。举个例子，比较说我管理着400多台mysql主机，现在由于一些安全方面的问题要对这400多台主机上的mysql进行升级；如果个400台主机上的mysql它们的安装目录、配置文件、都各有各的特色；
比如有的安装在/usr/local/mysql，有的安装在/home/mysql/mysql/，有的又安装在/var/mysql/，这样的话就不能用同一个升级步骤去应对所有的主机，这就意味着不能批量的对这400台主机做升级了。

## 安装与配置mysqltools
mysqltools 提供的自动化，集中化运维能力是建立在ansible的基础之上，所以安装ansible 就成了使用mysqltools先决条件；
ansible 这个软件又是由python写出来的，实际上绝大部分linux操作系统都已经安装上了python2.x，作为一个面向未来的软件
mysqltools并没有使用python2.x而是基于python3.6.x上开发完成的。所以在你安装ansible之前还要先安装上python.3.6.x
好在所以的安装包mysqltool都已经为你准备好了，mysqltools/deploy/packages/目录下；不只是这样，为们还把安装流程写成
了脚本，这样你就只要运行一下mysqltools给出的安装脚本就能自动化安装mysqltools了。

### 安装python
为了方便离线安装python3.6.x 的安装包已经打包到了mysqltools/deploy/packages/python中 注意安装的过程要用root用户

#### 安装python第一步
安装python3.6.x 的相关依赖包

    yum -y install gcc gcc-c++ libffi libffi-devel zlib zlib-devel openssl openssl-devel libyaml sqlite-devel libxml2 libxslt-devel libxml2-devel

#### 安装python第二步
安装python-3.6.x 

    cd mysqltools/deploy/packages/python
    tar -xvf python-3.6.2.tar.xz -C /tmp/
    cd /tmp/Python-3.6.2/
    ./configure --prefix=/usr/local/python-3.6.2/
    make -j 2
    make install
    cd /usr/local/
    ln -s /usr/local/python-3.6.2  python
    echo 'export PATH=/usr/local/python/bin/:$PATH' >> /etc/profile
    source /etc/profile

#### 自动化安装python
事实上mysqltools/deplay/packages/python/install.sh 脚本中包涵了上面两个步骤的命令可以用root用户直接运它以完成python的安装

    bash install.sh

### 安装ansible
为了方便离线安装 ansible-2.4.0.0 的安装包和与之相关的依赖包都已经保存到mysqltool/deploy/packages/ansible目录下


#### 安装ansibe第一步解决依赖问题

##### 安装pycparser
    cd mysqltool/deploy/packages/ansible
    tar -xvf pycparser-2.18.tar.gz -C /tmp/
    cd /tmp/pycparser-2.18
    python3 setup.py build
    python3 setup.py install

##### 安装six
    cd mysqltool/deploy/packages/ansible
    tar -xvf six-1.11.0.tar.gz -C /tmp/
    cd /tmp/six-1.11.0
    python3 setup.py build
    python3 setup.py install

##### 安装asn1crypto
    cd mysqltool/deploy/packages/ansible
    tar -xvf asn1crypto-0.23.0.tar.gz -C /tmp/
    cd /tmp/asn1crypto-0.23.0
    python3 setup.py build
    python3 setup.py install

##### 安装idna
    cd mysqltool/deploy/packages/ansible
    tar -xvf idna-2.6.tar.gz -C /tmp/
    cd /tmp/idna-2.6
    python3 setup.py build
    python3 setup.py install

##### 安装cryptography
    cd mysqltool/deploy/packages/ansible
    tar -xvf cryptography-2.1.1.tar.gz -C /tmp/
    cd /tmp/cryptography-2.1.1/
    python3 setup.py build
    python3 setup.py install

##### 安装pyasn1
    cd mysqltool/deploy/packages/ansible
    tar -xvf pyasn1-0.3.7.tar.gz -C /tmp/
    cd /tmp/pyasn1-0.3.7
    python3 setup.py build
    python3 setup.py install

##### 安装PyNaCl
    cd mysqltool/deploy/packages/ansible
    tar -xvf PyNaCl-1.1.2.tar.gz -C /tmp/
    cd /tmp/PyNaCl-1.1.2
    python3 setup.py build
    python3 setup.py install 

##### 安装bcrypt
    cd mysqltool/deploy/packages/ansible
    tar -xvf bcrypt-3.1.4.tar.gz -C /tmp/
    cd /tmp/bcrypt-3.1.4 
    python3 setup.py build
    python3 setup.py install 

##### 安装paramiko
    cd mysqltool/deploy/packages/ansible
    tar -xvf paramiko-2.3.1.tar.gz -C /tmp/
    cd /tmp/paramiko-2.3.1
    python3 setup.py build
    python3 setup.py install 

##### 安装PyYAML
    cd mysqltool/deploy/packages/ansible
    tar -xvf PyYAML-3.12.tar.gz -C /tmp/
    cd /tmp/PyYAML-3.12
    python3 setup.py build
    python3 setup.py install 

##### 安装MarkupSafe
    cd mysqltool/deploy/packages/ansible
    tar -xvf MarkupSafe-1.0.tar.gz -C /tmp/
    cd /tmp/MarkupSafe-1.0
    python3 setup.py build
    python3 setup.py install    

##### 安装Jinja2
    cd mysqltool/deploy/packages/ansible
    tar -xvf Jinja2-2.9.6.tar.gz -C /tmp/
    cd/tmp/Jinja2-2.9.6
    python3 setup.py build
    python3 setup.py install 

#### 安装ansibe第二步安装ansible
    cd mysqltools/deploy/packages/ansible/
    tar -xvf ansible-2.4.0.0.tar.gz -C /tmp/
    python3 setup.py build
    python3 setup.py install 

#### 自动化安装ansible
作为一个着眼于自动化的工具当然是不应该有这么困难的安装方式的，mysqltools为自己写好自动化安装的脚本，注意这个要用root身份运行

    cd mysqltools/deploy/package/ansible
    bash install.sh

# mysqltools快速入门
安装完python 、ansible 对于mysqltools就已经具备可运行的基础了；你还要配置两个配置项，这样你的mysqltools就能正常运行了。

## 配置mysqltools
- 1 mysqltools 的全局配置文件保存在mysqltools/deploy/ansible/std_vars.yaml文件中；所以配置mysqltools就只要改这个文件就行了

- 2 mtls_base_dir 这个变量是mysqltools的基准目录、它的值应该是你进入mysqltools这个上工具后pwd命令所输出的值；注意这人目录要以'/'结尾
    >例如：你把mysqltools 下载到了/tmp/目录下、那么mtls_base_dir的值就应该是/tmp/mysqltools/

- 3 mysql_packages_dir 由于mysql的安装包过于巨大、所以mysqltools中并没有内置mysql的安装包；mysql的安装包你要自己去官网下载，
然而mysql_packages_dir 就是mysql安装包所在的路径。
    >例如：你把mysql的安装包下载到了/opt/softwars/mysql/目录下，那么mysql_packages_dir的值就是/opt/softwars/mysql/
    最新mysql-5.7二进制包的下载地址：https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.20-linux-glibc2.12-x86_64.tar.gz

## 使用mysqltools
由于mysqltools是基于ansible开发出为的工具集、所以要熟练的使用mysqltools你要先了解一下ansible
- 1 ansible中文文档：http://www.ansible.com.cn/index.html
- 2 ansible英文文档：http://docs.ansible.com/ansible/latest/index.html

# mysqltools功能列表

## mysql安装
### 单机实例mysql的安装
- 1 进入mysql工具所在的目录

        cd mysqltools/deploy/ansible/mysql/

- 2 设置install_single_mysql.yaml文件中的hosts字段的值为你要执行mysql包安装的目标机器

- 3 调用ansible-playbook完成自动化安装过程

        ansible-playbook install_single_mysql.yaml 
        PLAY [cstudio] ****************************************************************************
        TASK [Gathering Facts] ********************************************************************
        ok: [cstudio]
        TASK [create mysql user] ******************************************************************
        changed: [cstudio]
        TASK [create and config /etc/my.cnf] ******************************************************
        changed: [cstudio]
        TASK [transfer mysql install package to remote host and unarchive to /usr/local/] *********
        changed: [cstudio]
        TASK [change owner to mysql user] *********************************************************
        changed: [cstudio]
        TASK [make link /usr/local/mysql-xx.yy.zz to /usr/local/mysql] ****************************
        changed: [cstudio]
        TASK [export mysql share object (*.os)] ***************************************************
        changed: [cstudio]
        TASK [load share object] ******************************************************************
        changed: [cstudio]
        TASK [export path env variable] ***********************************************************
        changed: [cstudio]
        TASK [export path env to /root/.bashrc] ***************************************************
        changed: [cstudio]
        TASK [make link /usr/local/mysql-xx.yy.zz to /usr/local/mysql] ****************************
        changed: [cstudio]
        TASK [create datadir] *********************************************************************
        changed: [cstudio]
        TASK [initialize-insecure] ****************************************************************
        changed: [cstudio]
        TASK [create libmysqlclient_r.so file for php-5.6] ****************************************
        changed: [cstudio]
        TASK [create systemd config file] *********************************************************
        changed: [cstudio]
        TASK [enable mysqld service] **************************************************************
        changed: [cstudio]
        TASK [start mysql(sytemctl)] **************************************************************
        changed: [cstudio]
        TASK [config mysql.service start up on boot] **********************************************
        changed: [cstudio]
        TASK [config sysv start script] ***********************************************************
        skipping: [cstudio]
        TASK [start mysql(service)] ***************************************************************
        skipping: [cstudio]
        TASK [config mysql.service start up on boot] **********************************************
        skipping: [cstudio]
        TASK [transfer sql statement to remonte] **************************************************
        changed: [cstudio]
        TASK [make mysql secure] ******************************************************************
        changed: [cstudio]
        TASK [clear /tmp/ directory] **************************************************************
        changed: [cstudio]
        PLAY RECAP ********************************************************************************
        cstudio                    : ok=21   changed=20   unreachable=0    failed=0 

- 4 测试mysql数据是否安装成功

        [root@cstudio data]# mysql -uroot -pmtls0352
        mysql: [Warning] Using a password on the command line interface can be insecure.
        Welcome to the MySQL monitor.  Commands end with ; or \g.
        Your MySQL connection id is 5
        Server version: 5.7.20-log MySQL Community Server (GPL)
        
        Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.
        
        Oracle is a registered trademark of Oracle Corporation and/or its
        affiliates. Other names may be trademarks of their respective
        owners.
        
        Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
        
        mysql> 

- 5 注意事项：
    - 1 如果你的目标端操作系统是linux-6.x 而且是最小化安装的情况下会有两个问题出现、原因是缺少selinux-python、numactl
    不过可以通过
    
            yum -y install selinux-python numactl
        把它们行安装一下

## 被控主机上的python安装
这里介绍的python的安装与前面介绍的[安装python](#安装python)所面向的问题是不一样的、[安装python](#安装python)是为了
在主控机上安装ansible,mysqltools才安装的python；这里介绍的python安装是在已经安装完成ansible之后，在被控主机上安装
python。之所以要在被控机上安装python是因为mysqltools的大多数功能是用python写的，比如对mysql进行监控时，mysql各项
指标的收集工作是通过python语言来实现的。

- 1 进入安装python的playbook所在的目录

        cd mysqltools/deploy/ansible/python

- 2 修改install_python.yaml文件中的hosts变量为你要安装的主机

- 3 执行安装

        ansible-playbook install_python.yaml 
        PLAY [cstudio] ************************************************************************
        TASK [Gathering Facts] ****************************************************************
        ok: [cstudio]
        TASK [install gcc] ********************************************************************
        ok: [cstudio]
        TASK [install gcc-c++] ****************************************************************
        ok: [cstudio]
        TASK [install libyaml-devel] **********************************************************
        ok: [cstudio]
        TASK [install libffi-devel] ***********************************************************
        ok: [cstudio]
        TASK [install zlib-devel] *************************************************************
        ok: [cstudio]
        TASK [install openssl-devel] **********************************************************
        ok: [cstudio]
        TASK [install sqlite-devel] ***********************************************************
        ok: [cstudio]
        TASK [install libxslt-devel] **********************************************************
        ok: [cstudio]
        TASK [install libxml2-devel] **********************************************************
        ok: [cstudio]
        TASK [transfer python-3.6.2.tar.x zpackage to remonte host] ***************************
        changed: [cstudio]
        TASK [transfer python install script to remonte host /tmp/] ***************************
        changed: [cstudio]
        TASK [install python] *****************************************************************
        changed: [cstudio]
        TASK [create link file] ***************************************************************
        changed: [cstudio]
        TASK [export path env variable(/etc/profile)] *****************************************
        ok: [cstudio]
        TASK [export path env variable(/root/.bashrc)] ****************************************
        ok: [cstudio]
        TASK [remove /tmp/install_python.sh] **************************************************
        changed: [cstudio]
        TASK [remove /tmp/Python-3.6.2] *******************************************************
        changed: [cstudio]
        TASK [transfer mysql-connector-python-2.1.5.tar.gz to remonte host] *******************
        ok: [cstudio]
        TASK [transfer mysql-connector-python install script to remonte host] *****************
        ok: [cstudio]
        TASK [install mysql-connector-python] *************************************************
        changed: [cstudio]
        TASK [remove tmp/install_mysql_connector_python.sh] ***********************************
        ok: [cstudio]
        PLAY RECAP ****************************************************************************
        cstudio                    : ok=22   changed=7    unreachable=0    failed=0 

- 4 测试python3有没有安装成功

        python3
        Python 3.6.2 (default, Nov  3 2017, 14:09:03) 
        [GCC 4.8.5 20150623 (Red Hat 4.8.5-4)] on linux
        Type "help", "copyright", "credits" or "license" for more information.
        >>> import mysql
        >>> print("hello mtls")
        hello mtls
        >>> 

 - 5 注意事项
 >由于mysqltools主要是解决mysql相关的问题、解决问题用到的语言是python、目前mysql官方的python连接mysql驱动
 包就是mysql-connector-python 所以mysqltools会在安装python的同时也把这个包也安装上；当然你也可以通过设置
 std_vars.yaml配置文件中mtls_with_mysql_conntor_python的值为0 来禁止这一操作
        
## mysql监控环境的安装
对于mysql的监控mysqltools采用国际一流的开源解决方案(zabbix)来实现、各项监控指标会由zabbix_agent完成收集、并发往zabbix_server、在zabbix_server收到数据后会做一些动作如：数据超过事先设定阈值时会告警，对于每一项收到到的数据zabbix_server都
会把它保存到zabbix自用的后台的数据库中；zabbix为了方便使用还给用户配了一个web界面；当然这个web界面的所有数据都来自于zabbix自用的后台的数据库。这里的介绍有些片面，只是因为我在这里想表达的重点是zabbix环境的建设是在<strong style="color:red;">LAMP</strong>的基础上搞出来的；所以要建设zabbix监控环境
就要先把<strong style="color:red;">LAMP</strong>搭建起来。

### 安装zabbix自用的后台mysql数据库
这个可以参照 [单机实例mysql的安装](#单机实例mysql的安装)

### httpd的安装
mysqltools已经把httpd的源码包都打包进来了，只要简单的两步就能完成httpd的安装

- 1 进入安装httpd的playbook所在的目录

        cd mysqltools/deploy/ansible/httpd/

- 2 修改install_httpd.yaml文件中的hosts变量为你要安装的主机

- 3 执行安装
        
        ansible-playbook install_httpd.yaml 
        PLAY [cstudio] ****************************************************************************
        TASK [Gathering Facts] ********************************************************************
        ok: [cstudio]
        TASK [install gcc] ************************************************************************
        ok: [cstudio]
        TASK [install gcc-c++] ********************************************************************
        ok: [cstudio]
        TASK [install pcre-devel] *****************************************************************
        ok: [cstudio]
        TASK [openssl-devel] **********************************************************************
        ok: [cstudio]
        TASK [expat-devel] ************************************************************************
        ok: [cstudio]
        TASK [transfer apr-1.6.2.tar.gz to remote host] *******************************************
        changed: [cstudio]
        TASK [copy install script to remote] ******************************************************
        changed: [cstudio]
        TASK [install apr] ************************************************************************
        changed: [cstudio]
        TASK [remove /tmp/install_apr.sh] *********************************************************
        changed: [cstudio]
        TASK [remove /tmp/apr-1.6.2] **************************************************************
        changed: [cstudio]
        TASK [transfer apr-util-1.6.0.tar.gz to remote host] **************************************
        changed: [cstudio]
        TASK [copy install script to remote] ******************************************************
        changed: [cstudio]
        TASK [install apr_util] *******************************************************************
        changed: [cstudio]
        TASK [clear /tmp/ directory] **************************************************************
        changed: [cstudio]
        TASK [clear /tmp/ directory] **************************************************************
        changed: [cstudio]
        TASK [copy httpd-2.4.28.tar.gz to remonte host] *******************************************
        changed: [cstudio]
        TASK [copy install scripts to remonte host] ***********************************************
        changed: [cstudio]
        TASK [install httpd] **********************************************************************
        changed: [cstudio]
        TASK [copy httpd.conf to remonte host] ****************************************************
        changed: [cstudio]
        TASK [config sysctl] **********************************************************************
        changed: [cstudio]
        TASK [start httpd] ************************************************************************
        changed: [cstudio]
        TASK [start httpd] ************************************************************************
        skipping: [cstudio]
        TASK [config sysv start script(only linux-6)] *********************************************
        skipping: [cstudio]
        TASK [config httpd start up on boot(only linux-6)] ****************************************
        skipping: [cstudio]
        TASK [start httpd(only linux-6)] **********************************************************
        skipping: [cstudio]
        TASK [remove /tmp/install_httpd.sh] *******************************************************
        changed: [cstudio]
        TASK [remove /tmp/httpd-2.4.28.tar.gz] ****************************************************
        changed: [cstudio]
        PLAY RECAP ********************************************************************************
        cstudio                    : ok=24   changed=18   unreachable=0    failed=0
        
### php的安装
mysqltools会把php安装成httpd的一个模块

- 1 进入到安装php的playbook的目录

        cd mysqltools/deploy/ansible/php

- 2 修改install_php.yaml文件中的hosts变量为你要安装的主机

- 3 执行安装

        ansible-playbook install_php.yaml 
        PLAY [cstudio] ****************************************************************************
        TASK [Gathering Facts] ********************************************************************
        ok: [cstudio]
        TASK [install gcc] ************************************************************************
        ok: [cstudio]
        TASK [install gcc-c++] ********************************************************************
        ok: [cstudio]
        TASK [install bzip2-devel] ****************************************************************
        changed: [cstudio]
        TASK [install libjpeg-devel] **************************************************************
        changed: [cstudio]
        TASK [install libpng-devel] ***************************************************************
        changed: [cstudio]
        TASK [install freetype-devel] *************************************************************
        changed: [cstudio]
        TASK [install freetype-devel] *************************************************************
        ok: [cstudio]
        TASK [copy and untar php-5.6.31.tar.gz to remonte host] ***********************************
        changed: [cstudio]
        TASK [copy install_php.sh to remonte host] ************************************************
        changed: [cstudio]
        TASK [install php] ************************************************************************
        changed: [cstudio]
        TASK [copy php.ini to remote] *************************************************************
        changed: [cstudio]
        TASK [remove /tmp/install_php.sh] *********************************************************
        changed: [cstudio]
        TASK [remove /tmp/php-5.6.31] *************************************************************
        changed: [cstudio]
        PLAY RECAP ********************************************************************************
        cstudio                    : ok=14   changed=10   unreachable=0    failed=0 


### zabbix-server的安装
- 1 进入安装zabbix-server的目录

        cd mysqltools/deplay/ansible/zabbix/

- 2 修改nstall_zabbix_server.yaml 文件中的hosts变量为你要安装的主机

- 3 执行安装脚本

        ansible-playbook install_zabbix_server.yaml 
        PLAY [cstudio] ****************************************************************************
        TASK [Gathering Facts] ********************************************************************
        ok: [cstudio]
        TASK [add zabbix user to system] **********************************************************
        ok: [cstudio]
        TASK [remove /usr/local/httpd/htdocs/index.html] ******************************************
        ok: [cstudio]
        TASK [install gcc] ************************************************************************
        ok: [cstudio]
        TASK [install gcc-c++] ********************************************************************
        ok: [cstudio]
        TASK [install libxml2-devel] **************************************************************
        ok: [cstudio]
        TASK [install curl-devel] *****************************************************************
        ok: [cstudio]
        TASK [install unixODBC-devel] *************************************************************
        ok: [cstudio]
        TASK [install net-snmp-devel] *************************************************************
        ok: [cstudio]
        TASK [install OpenIPMI-devel] *************************************************************
        ok: [cstudio]
        TASK [install libevent-devel] *************************************************************
        ok: [cstudio]
        TASK [transfer zabbix install package to remote host and unarchive to /tmp/] **************
        changed: [cstudio]
        TASK [transfer install script to remonte host] ********************************************
        ok: [cstudio]
        TASK [install zabbix_server_node] *********************************************************
        changed: [cstudio]
        TASK [copy zabbix web-site file to /usr/local/httpd/htdocs/] ******************************
        changed: [cstudio]
        TASK [change /usr/local/httpd/htdocs/ owner and group] ************************************
        changed: [cstudio]
        TASK [change owner to zabbix user] ********************************************************
        changed: [cstudio]
        TASK [make link] **************************************************************************
        changed: [cstudio]
        TASK [transfer zabbix config file to remonte host] ****************************************
        changed: [cstudio]
        TASK [transfer zabbix database init script to remonte host] *******************************
        changed: [cstudio]
        TASK [init zabbix database] ***************************************************************
        changed: [cstudio]
        TASK [remove /tmp/install_zabbix_server.sh] ***********************************************
        changed: [cstudio]
        TASK [remove /tmp/zabbix-3.4.3] ***********************************************************
        changed: [cstudio]
        TASK [remove /tmp/init_zabbix_database.sql] ***********************************************
        changed: [cstudio]
        TASK [stop httpd(linux-6)] ****************************************************************
        skipping: [cstudio]
        TASK [stop httpd(linux-7)] ****************************************************************
        ok: [cstudio]
        TASK [start zabbix-server] ****************************************************************
        changed: [cstudio]
        TASK [start zabbix-agent(on zabbix-server host)] ******************************************
        changed: [cstudio]
        PLAY RECAP ********************************************************************************
        cstudio                    : ok=27   changed=14   unreachable=0    failed=0 
    
- 4 通过浏览器测试zabbix-server是否安装成功
![](docs/imgs/zabbix_start_page_0001.png)
![](docs/imgs/zabbix_start_page_0002.png)
![](docs/imgs/zabbix_start_page_0003.png)
![](docs/imgs/zabbix_main_page_0001.png)

### zabbix-agent的安装

- 1 进入安装zabbix-agent的目录

        cd mysqltools/deplay/ansible/zabbix/

- 2 修改nstall_zabbix_agent.yaml 文件中的hosts变量为你要安装的主机，由于zabbix_agent还要与zabbix_server间进行
通信，所以在安装zabbix_agent时还要告诉它zabbix-server的IP地址，这个地址值可以在std_vars.yaml文件中的zabbix_server_ip配置项指定。

- 3 执行安装脚本

        ansible-playbook install_zabbix_agent.yaml 
        PLAY [cstudio] ****************************************************************************
        TASK [Gathering Facts] ********************************************************************
        ok: [cstudio]
        TASK [add zabbix user to system] **********************************************************
        ok: [cstudio]
        TASK [install gcc] ************************************************************************
        ok: [cstudio]
        TASK [install gcc-c++] ********************************************************************
        ok: [cstudio]
        TASK [install libxml2-devel] **************************************************************
        ok: [cstudio]
        TASK [install curl-devel] *****************************************************************
        ok: [cstudio]
        TASK [install unixODBC-devel] *************************************************************
        ok: [cstudio]
        TASK [install net-snmp-devel] *************************************************************
        ok: [cstudio]
        TASK [install OpenIPMI-devel] *************************************************************
        ok: [cstudio]
        TASK [install libevent-devel] *************************************************************
        ok: [cstudio]
        TASK [transfer zabbix install package to remote host and unarchive to /tmp/] **************
        changed: [cstudio]
        TASK [transfer install script to remonte host] ********************************************
        changed: [cstudio]
        TASK [install zabbix_agent_node] **********************************************************
        changed: [cstudio]
        TASK [change owner to zabbix user] ********************************************************
        changed: [cstudio]
        TASK [make link] **************************************************************************
        changed: [cstudio]
        TASK [transfer zabbix config file to remonte host] ****************************************
        changed: [cstudio]
        TASK [remove /tmp/install_zabbix_agent.sh] ************************************************
        changed: [cstudio]
        TASK [remove /tmp/zabbix-3.4.3] ***********************************************************
        changed: [cstudio]
        TASK [start zabbix-agent] *****************************************************************
        changed: [cstudio]
        PLAY RECAP ********************************************************************************
        cstudio                    : ok=19   changed=9    unreachable=0    failed=0 

- 4 查看abbix-agent 是否正常运行

        ps -ef | grep zabbix                                                                   
        zabbix    89267      1  0 15:46 ?        00:00:00 /usr/local/zabbix/sbin/zabbix_agentd                       
        zabbix    89268  89267  0 15:46 ?        00:00:00 /usr/local/zabbix/sbin/zabbix_agentd: collector [idle 1 sec]
        zabbix    89269  89267  0 15:46 ?        00:00:00 /usr/local/zabbix/sbin/zabbix_agentd: listener #1 [waiting for connection]
        zabbix    89270  89267  0 15:46 ?        00:00:00 /usr/local/zabbix/sbin/zabbix_agentd: listener #2 [waiting for connection]
        zabbix    89271  89267  0 15:46 ?        00:00:00 /usr/local/zabbix/sbin/zabbix_agentd: listener #3 [waiting for connection]
        zabbix    89272  89267  0 15:46 ?        00:00:00 /usr/local/zabbix/sbin/zabbix_agentd: active checks #1 [idle 1 sec]
        zabbix    89273  89267  0 15:46 ?        00:00:00 /usr/local/zabbix/sbin/zabbix_agentd: active checks #2 [idle 1 sec]