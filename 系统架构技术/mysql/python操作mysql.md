# python 操作 mysql
    - yum install MySQL-python -y  安装MySQLdb

    - python2.7没有对应版本的mysqldb
        - apt-get install python-mysqldb
        - apt-get install python-all-dev
        - apt-get install libmysqlclient15-dev
        - apt-get install zlib1g-dev
            - 这样django就可以运行了

    - pip install mysql-python (windows7 64bit 安装)
        - error: Microsoft Visual C++ 9.0 is required. Get it from http://aka.ms/vcpython27
        - 访问 http://aka.ms/vcpython27 跳转到下面下载链接
        - http://www.microsoft.com/en-us/download/details.aspx?id=44266
        - 错误：  fatal error C1083: Cannot open include file: 'config-win.h'
        - 下载 https://dev.mysql.com/downloads/connector/c/6.0.html#downloads
        - 目录命名为 C:\Program Files (x86)\MySQL\MySQL Connector C 6.0.2
        - 新建目录 C:\Program Files (x86)\MySQL\MySQL Connector C 6.0.2\lib\opt  ; 并复制lib文件到该目录
        -

    - 编码问题 utf8
        - 文件开头加入说明，任选一种
            - #-*- coding: UTF-8 -*-
            - #coding=utf-8
        - 数据库连接配置，任选一种
            - db.set_character_set('utf8')
            - cursor.execute('SET NAMES utf8;')
        -
    - ### [SQLAlchemy 和其他的 ORM 框架](http://python.jobbole.com/84100/)

- python ORM peewee 安装：
        pip install peewee
    - ### 

## [peewee](http://docs.peewee-orm.com/en/latest/index.html)

- pip install peewee

- 常用
    - database = MySQLDatabase('userservicetest', **{'host': '172.21.12.120', 'password': 'tmp', 'user': 'tmp', 'charset':'utf8'})
    - python -m pwiz -e mysql -H 172.21.12.120 --user=tmp -P userservicetest > userservicetest.py
    -
    -
