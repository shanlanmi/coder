----
title: Mac下使用homebrew安装postgresql及配置
date: 2017-01-03 20:16:15
categories:
- DevDependent
----
安装 postgreSql

    brew install postgresql 

创建postgreSql数据库: 

    initdb /usr/local/var/postgres 

启动服务： 

    pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start 

停止服务: 

    pg_ctl -D /usr/local/var/postgres stop -s -m fast 

自动启动服务: 

    mkdir -p ~/Library/LaunchAgents  
    cp /usr/local/Cellar/postgresql/9.2.4/homebrew.mxcl.postgresql.plist ~/Library/LaunchAgents/launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist 

删除自动启动服务: 

    launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist

好了，现在基本就算完成了，很简单。这时候下载pgadmin3，安装之后会出现**role 'postgres' does not exist**的错误，原因你是没有创建postgres用户。 

    createuser -d -a -P postgres 

执行完这行命令后，postgresl角色就创建好了，再连接就不会报错。 

导入sql文件，如导入back.sql：

    psql -d database_name -f back.sql 

在linux下需要首先切换到postgres用户再执行该命令: 

    su postgres 

导出数据库到文件: 

    pg_dump database_name > back.sql 

卸载postgresql,如果是使用homebrew安装的话，就和简单了: 

    brew uninstall postgresql 

如果是下载安装包安装的，有两种方法。  
1. 自动卸载，在安装目录下，mac下是/Applications/Postgresql下有个uninstall-postgresql.app，双击执行就可以了。  
2. 手动删除。 

* 停止服务

    sudo /sbin/SystemStarter stop postgresql-9.2 

* 移除菜单图标

    sudo rm -rf /Applications/PostgreSQL 9.2 

* 移除ini文件

    sudo rm -rf /etc/postgres-reg.ini 

* 移除startup items

    sudo rm -rf /Library/StartupItems/postgresql-9.2 

* 移除数据和安装文件

    sudo rm -rf /Library/PostgreSQL/9.2 

* 移除postgres用户

    sudo dscl . delete /users/postgres 

PS:   
可参考这两篇blog  
1. http://nextmarvel.net/blog/2011/09/brew-install-postgresql-on-os-x-lion/  
2. http://kidsreturn.org/2012/03/install-postgresql-on-mac-lion-via-homebrew/
