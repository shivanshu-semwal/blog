---
title: "Install MySql on Ubuntu"
layout: post
tags:
  - linux
  - mysql
  - sql
description: Installing and securing MySQL on Ubuntu, including user setup and password recovery.
image:
  path: /assets/img/covers/installing-mysql-on-ubuntu-cover.png
  alt: "Installing MySql on Ubuntu"
---


> for version 8.0.27

* First install `my-sql` database:
    
    ```sh
    sudo apt install mysql-server
    # start the utility prompt to set the root user password
    sudo mysql_secure_installation utility
    ```
    
* Allow remote access
    
    ```sh
    sudo ufw enable
    sudo ufw allow mysql
    ```
    
* Start the sql service
    
    ```sh
    sudo systemctl start mysql
    # to allow the service to start at startup
    sudo systemctl enable mysql
    ```
    
* Start mysql shell
    
    ```sh
    /usr/bin/mysql -u root -p
    ```
    
* View users
    
    * `SELECT User, Host, authentication_string FROM mysql.user;`
        
* Add a new user
    
    * `CREATE USER 'new_user'@'`[`localhost`](http://localhost)`' IDENTIFIED BY 'new_password';`
        
    * [localhost](http://localhost) for local user
        
* gGive permission to the user to access some database
    
    * `GRANT ALL ON database_name.* TO 'user_name'@'`[`localhost`](http://localhost)`';`
        
    * do `FLUSH PRIVILEGES;` to reload the assigned permissions
        

## Troubleshooting

* If you face any permission issues while launching the `mysql`
    
    * use `chown -R mysql:mysql filename`
        
* Start `mysql` with `skip-grant-tables`
    
    * `mysqld --skip-grant-permissions --user=root`
        

## Resetting the root user password

* If your data directory `/var/lib/mysql` contains the old version `mysql` format data, or something is wrong with it, regenerate it
    
    ```bash
    # Move or remove the original dir
    mv /var/lib/mysql /tmp/mysql
    
    # Create a new dir
    mkdir /var/lib/mysql
    
    # Change owner to mysql (user and group)
    chown -R mysql:mysql /var/lib/mysql
    
    # Create MySQL initial data
    mysqld --initialize
    ```
    
* Now start the `mysqld` with `skip grant tables`
    
    ```bash
    # starting the mysqld daemon
    sudo mysqld --skip-grant-tables --user=root
    # or if you can't open another terminal
    sudo mysqld --skip-grant-tables --user=root &
    
    # open mysql
    mysql -u root
    ```
    
* Fix the user permission table
    
    ```sql
    UPDATE mysql.user SET authentication_string=null WHERE User='root';
    FLUSH PRIVILEGES;
    ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'yourpasswd';
    FLUSH PRIVILEGES;
    EXIT;
    ```
    
* Now you can login with the new password.
    

## References

* [https://dba.stackexchange.com/questions/103625/how-to-reinitialise-var-lib-mysql-files](https://dba.stackexchange.com/questions/103625/how-to-reinitialise-var-lib-mysql-files)
    
* [https://stackoverflow.com/questions/50691977/how-to-reset-the-root-password-in-mysql-8-0-11](https://stackoverflow.com/questions/50691977/how-to-reset-the-root-password-in-mysql-8-0-11)
    
* [https://stackoverflow.com/questions/57603349/errormysqld-service-start-request-repeated-too-quickly-on-manjaro](https://stackoverflow.com/questions/57603349/errormysqld-service-start-request-repeated-too-quickly-on-manjaro)
    
* [https://superuser.com/questions/603026/mysql-how-to-fix-access-denied-for-user-rootlocalhost](https://superuser.com/questions/603026/mysql-how-to-fix-access-denied-for-user-rootlocalhost)
