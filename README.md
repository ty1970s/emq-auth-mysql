
## Overview

emqttd Authentication, ACL with MySQL Database

## Build Plugin

This project is a plugin for emqttd broker.

In emqttd project:

If the submodule exists:

```
git submodule update --remote
```

Orelse:

```
git submodule add https://github.com/erylee/emysql.git plugins/emysql

git submodule add https://github.com/emqtt/emqttd_plugin_mysql.git plugins/emqttd_plugin_mysql

make && make dist
```

## Configure Plugin

File: etc/plugin.config

```erlang
[
{emysql, [
    {pool, 4},
        {host, "localhost"},
        {port, 3306},
        {username, "root"},
        {password, "root"},
        {database, "emqtt"},
        {encoding, utf8}
]},
{emqttd_plugin_mysql, [
    {user_table, mqtt_users},
    %% plain, md5, sha
    {password_hash, plain},
    {field_mapper, [
       {username, username},
       {password, password}
    ]}
]}
].
```

## Load Plugin

```
./bin/emqttd_ctl plugins load emqttd_plugin_mysql
```

## Customize Plugin

Fork this project and implement your own authentication/ACL mechanism.


## Users Table(Demo)

Notice: This is a demo table. You could authenticate with any user tables.

    ```
    CREATE TABLE auth_user (
            id int(11) NOT NULL AUTO_INCREMENT,
            password varchar(128) NOT NULL,
            is_superuser tinyint(1) NOT NULL,
            username varchar(30) NOT NULL,
            PRIMARY KEY (id),
            UNIQUE KEY username (username)
            ) ENGINE=InnoDB AUTO_INCREMENT=17 DEFAULT CHARSET=utf8
    CREATE TABLE auth_acl (
            id int(11) NOT NULL AUTO_INCREMENT,
            topic varchar(100) NOT NULL,
            username varchar(30) NOT NULL,
            rw tinyint(1) NOT NULL,
            PRIMARY KEY (id)
            ) ENGINE=InnoDB AUTO_INCREMENT=66 DEFAULT CHARSET=utf8

    ```
