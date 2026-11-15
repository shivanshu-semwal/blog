---
title: "Install MySql on Ubuntu"
layout: post
tags:
  - linux
  - mysql
  - sql
description: Installing and securing MySQL on Ubuntu, including user setup and password recovery.
---

> For MySQL version 8.0.27

* Install the MySQL server:

```bash
sudo apt install mysql-server
sudo mysql_secure_installation utility
```

* Allow remote access:

```bash
sudo ufw enable
sudo ufw allow mysql
```

* Start the MySQL service:

```bash
sudo systemctl start mysql
sudo systemctl enable mysql
```

* Start the MySQL shell:

```bash
/usr/bin/mysql -u root -p
```

* View users:

```sql
SELECT User, Host, authentication_string FROM mysql.user;
```

* Add a new user:

```sql
CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'new_password';
```

* Grant permissions:

```sql
GRANT ALL ON database_name.* TO 'user_name'@'localhost';
FLUSH PRIVILEGES;
```

## Troubleshooting

If there are permission issues, use:

```bash
chown -R mysql:mysql filename
```

## Resetting the root password

If your data directory is stale or corrupted, rebuild it:

```bash
mv /var/lib/mysql /tmp/mysql
mkdir /var/lib/mysql
chown -R mysql:mysql /var/lib/mysql
mysqld --initialize
```

Then start MySQL with skip-grant tables and reset the password:

```bash
sudo mysqld --skip-grant-tables --user=root
mysql -u root

UPDATE mysql.user SET authentication_string = null WHERE User = 'root';
FLUSH PRIVILEGES;
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'yourpasswd';
FLUSH PRIVILEGES;
```

## References

- DBA Stack Exchange
- Stack Overflow MySQL password reset guide
- Superuser access denied guide
