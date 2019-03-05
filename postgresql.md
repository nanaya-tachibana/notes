# 安装
首先，安装PostgreSQL客户端。

``` shell
sudo apt-get install postgresql-client
```

然后，安装PostgreSQL服务器。

``` shell
sudo apt-get install postgresql
```

正常情况下，安装完成后，PostgreSQL服务器会自动在本机的5432端口开启。
如果还想安装图形管理界面，可以运行下面命令，但是本文不涉及这方面内容。

``` shell
sudo apt-get install pgadmin3
```

# 添加新用户和新数据库

初次安装后，默认生成一个名为postgres的数据库和一个名为postgres的数据库用户。这里需要注意的是，同时还生成了一个名为postgres的Linux系统用户。
下面，我们使用postgres用户，来生成其他用户和新数据库。好几种方法可以达到这个目的，这里介绍两种。

## PostgreSQL控制台。

首先，新建一个Linux新用户，可以取你想要的名字，这里为dbuser。

``` shell
sudo adduser dbuser
```

然后，切换到postgres用户。

``` shell
sudo su - postgres
```

下一步，使用psql命令登录PostgreSQL控制台。

``` shell
psql
```

这时相当于系统用户postgres以同名数据库用户的身份，登录数据库，这是不用输入密码的。如果一切正常，系统提示符会变为"postgres=#"，表示这时已经进入了数据库控制台。以下的命令都在控制台内完成。
第一件事是使用\password命令，为postgres用户设置一个密码。

``` shell
\password postgres
```

第二件事是创建数据库用户dbuser（刚才创建的是Linux系统用户），并设置密码。

``` shell
CREATE USER dbuser WITH PASSWORD 'password';
```

第三件事是创建用户数据库，这里为exampledb，并指定所有者为dbuser。

``` shell
CREATE DATABASE exampledb OWNER dbuser;
```

第四件事是将exampledb数据库的所有权限都赋予dbuser，否则dbuser只能登录控制台，没有任何数据库操作权限。

``` shell
GRANT ALL PRIVILEGES ON DATABASE exampledb to dbuser;
```

## shell命令行。

添加新用户和新数据库，除了在PostgreSQL控制台内，还可以在shell命令行下完成。这是因为PostgreSQL提供了命令行程序createuser和createdb。还是以新建用户dbuser和数据库exampledb为例。
首先，创建数据库用户dbuser，并指定其为超级用户。

``` shell
sudo -u postgres createuser --superuser dbuser
```

然后，登录数据库控制台，设置dbuser用户的密码，完成后退出控制台。

``` shell
sudo -u postgres psql
\password dbuser
\q
```

接着，在shell命令行下，创建数据库exampledb，并指定所有者为dbuser。

``` shell
sudo -u postgres createdb -O dbuser exampledb
```

# 登录数据库

添加新用户和新数据库以后，就要以新用户的名义登录数据库，这时使用的是psql命令。

``` shell
psql -U dbuser -d exampledb -h 127.0.0.1 -p 5432
```

上面命令的参数含义如下：-U指定用户，-d指定数据库，-h指定服务器，-p指定端口。
输入上面命令以后，系统会提示输入dbuser用户的密码。输入正确，就可以登录控制台了。
psql命令存在简写形式。如果当前Linux系统用户，同时也是PostgreSQL用户，则可以省略用户名（-U参数的部分）。举例来说，我的Linux系统用户名为ruanyf，且PostgreSQL数据库存在同名用户，则我以ruanyf身份登录Linux系统后，可以直接使用下面的命令登录数据库，且不需要密码。

``` shell
psql exampledb
```


此时，如果PostgreSQL内部还存在与当前系统用户同名的数据库，则连数据库名都可以省略。比如，假定存在一个叫做ruanyf的数据库，则直接键入psql就可以登录该数据库。

``` shell
psql
```

# 导出

导出数据库

``` shell
pg_dump -U dbuser -f db.sql exampledb
```

导出数据表

``` shell
pg_dump -U dbuser -t mytable -f table.sql exampledb
```

# 导入

导入数据库

``` shell
psql -d exampledb -f db.sql dbuser
```

导入数据表与导入数据库相同



# 资料来源
1. http://www.ruanyifeng.com/blog/2013/12/getting_started_with_postgresql.html
2. https://blog.csdn.net/allen_oscar/article/details/9061573
