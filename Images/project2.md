`
## Documentation for Project 2

### Documentation for Apache Installation

`sudo apt update`

`sudo apt install nginx`

`sudo systemctl status nginx`

![nginx installation status](.\nginx-status.png)

 `curl http://127.0.0.1:80`

![nginx terminal status](.\nginx-cmd-status.png)


Html Image for nginx default page

![nginx default page](.\nginx-html-status.png)


### Documentation for Mysql Installation

`sudo apt install mysql-server`

`sudo mysql`

`sudo mysql_secure_installation`

![mysql server config status](.\mysql-status.png)


## Documentation for php Installation

`sudo apt install php-fpm php-mysql`

`php -v`

![php status](.\php-status.png)


## Dcumentation for configuring Nginx to use php processor

`sudo mkdir /var/www/projectLEMP`

`sudo chown -R $USER:$USER /var/www/projectLEMP`

`sudo nano /etc/nginx/sites-available/projectLEMP.conf`

```
#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}

```

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

`sudo nginx -t`

`sudo unlink /etc/nginx/sites-enabled/default`

`sudo systemctl reload nginx`

`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`


`http://18.130.151.183:80`

![php status config](.\php-status-config.png)


## Documentation on testing php with nginx

`sudo nano /var/www/projectLEMP/info.php`

```
<?php
phpinfo();
```

http://18.130.151.183/info.php

![php html status](.\php-output-status.png)


## Documentation on retrieving data from mysql database with php

`sudo mysql`

`mysql> CREATE DATABASE `example_database`;`

`mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`

`mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';`

`mysql> exit`

`mysql -u example_user -p`

`mysql> SHOW DATABASES;`

![show database](.\database-status.png)

`mysql>CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT, content VARCHAR(255), PRIMARY KEY(item_id));`

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My second important item");`

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My third important item");`

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My fourth important item");`

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My fifth important item");`

`mysql>  SELECT * FROM example_database.todo_list;`

![show table](.\database.png)

`mysql> exit`

`nano /var/www/projectLEMP/todo_list.php`

```
<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}

```

http://18.169.237.75/todo_list.php

![show table on html](.\html-database-status.png)

