<center><font face="黑体"  size=6>Centos8安装mysql8</font></center>















### mysql下载和上传

* 从官网选择redhat，centos和redhat是同源的

  官网地址：https://dev.mysql.com/downloads/mysql/，选择第一个即可

* 通过ftp等方式将文件传到 云服务器或者虚拟机的 /opt目录下（你也可以选择其他的目录）

### 安装

* 在opt(你上传的目录)下ls查看一下，

  解压：tar -xvf 文件名选择其中四个：common、lib、client、server安装即可（没有debug的）

* 选择其中四个：common、lib、client、server安装即可（没有debug的）安装命令

  rpm -ivh 文件名 --nodeps --force（四个操作一遍）

* 初始化命令

  ```shell
  mysqld --initialize;
  chown mysql:mysql /var/lib/mysql -R;
  systemctl start mysqld.service;
  systemctl  enable mysqld;
  ```

  

* 安装好后用 rpm -qa｜ grep mysql查看安装成功与否，如果出现四个就成了



### 修改密码

* 上面安装之后的密码是系统默认的，通过cat /var/log/mysqld.log | grep password查看密码，冒号后面的就是密码

* 通过mysql - u root -p进入mysql进行密码修改，如果这种方式总是报错说明你密码输入错误，也可以通过无验证方式进入

  在/etc/my.cnf中mysqld后面添加一行skip-grant-tables，记得（：wq！），重启：systemctl restart mysqld

* 修改root密码

  ```shell
  ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
  ```

  注意：如果在执行该步骤的时候出现ERROR 1290 (HY000): The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement 错误。则执行下 flush privileges 命令，再执行该命令即可。

  改完密码，如果是用跳验证方式的话，记得删除那行添加的，并重启mysql

* 接下来接可以用你的密码进入mysql开始使用了



注：本文是为了新手方便安装mysql，仅限于centos8和mysql8，其他版本的centos和mysql安装方式有出入，请适当修改或者参考它处（如，systemctl在旧版中是使用server命令，老的mysql的user表结构也不一样，改密码的指令也有出入）