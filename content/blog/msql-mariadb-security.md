---
draft: true
author: "Robert den Harink"
date: 2017-12-01
title: MySQL / MariaDB Security best practices.
subtitle: ""
tags: [programming, database]
colour: light-green
excerpt: |
  Blah
---
## Secure install
After installing MySQL you should set a root password if you haven't already during installation. Disable remote root login by removing root accounts other than `127.0.0.1` and remove anonymous users and test databases.

```bash
mysql_secure_installation
```

## Configuration hardening
Bind the database to an address that is private enough in your network or only to localhost (loopback) if MySQL runs on the same host.

If the database is used accoss a network it could be bennefitial to change the default port from `3306` to something else.

Turn on logs, in case of attacks you see intrusion-related activities in the logs.

Disable `LOCAL INFILE`, this gives MySQL access to the filesystem, you most likely do not want this.

Set the appropriate file permission to the configuration file(s):
```bash
chmod 644 /etc/my.cnf
```
configuration example for the above:
```ini
...
[mysqld]
bind-address = <your_address_here>
Port = <some_other_port>
log=/var/log/mysql.log
local-infile=0
...
```

## Command line
Delete MySQL Shell history. This stores shell session history of commands performed, remove these files after you are done.

Don't run MySQL commands from the command-line. All commands are stored in a history file. An attacker could find passwords for the database there.

## Users
Define application specific users with limited privileges.

You can use the security plugins provided by [MySQL](https://dev.mysql.com/doc/refman/5.7/en/security-plugins.html)

Update your installation regularly
