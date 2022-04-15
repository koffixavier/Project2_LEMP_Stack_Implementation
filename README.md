# Project2_LEMP_Stack_Implementation
Project2.submission
## Step 1: Launch an ubuntu server in AWS
- Launch an ubuntu server in AWS and edit the inbound rules by adding a rule that allows traffic throught port 80
![Screen Shot 2022-04-10 at 10 56 00 PM](https://user-images.githubusercontent.com/101080172/162657585-5518a922-3345-42a5-acd4-d1206ebd4f2e.png)
## Step 2: Install nginx web server on your local terminal
- first update your server's package index `sudo apt update`
- install nginx `sudo apt install nginx`
- connect to port 80 `curl http://ocalhost:80`
- Enter your public IP into your webserver and should get a default page such as
![Screen Shot 2022-04-10 at 11 38 13 PM](https://user-images.githubusercontent.com/101080172/162661272-0e17e086-529a-4c09-92cb-30338208ad9a.png)
## Step 3: install mysql 
- In order to store and data, we need a database system such as mysql `sudo apt install mysql-server`
- then remove insecure default setting `sudo mysql_secure_installation`
## Step 4: install php
- to edit code and generate dynamic website we need to install php `sudo apt install php-fpm php-mysql`
## Step 5: congigure nginx to use php processor
- Create a root dictory for your domain `sudo mkdir /var/www/projectLEMP
- assign ownership of the directory `sudo chown -R $USER:$USER /var/www/projectLEMP`
- using nano text edictor, create a configuration  `sudo nano /etc/nginx/sites-available/projectLEMP`

- ```
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
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

  }
- Activate the configuration by linking to the config file from Nginx’s `sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`
- disable default Nginx host that is currently configured to listen on port 80 `sudo unlink /etc/nginx/sites-enabled/default`
- reload Nginx to apply the changes `sudo systemctl reload nginx`
- Create an index.html file that will serve as static website 

```
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```
- If you type your IP, you should get a page such ass
![Screen Shot 2022-04-09 at 4 09 23 PM](https://user-images.githubusercontent.com/101080172/163095730-23681192-e1c0-4f6a-b357-1610ba365eb7.png)

## Step 6: 
- creating a test PHP file in your document root `sudo mkdir /var/www/projectLEMP/info.php
- then add this code that will return information about your server 
```
<?php
phpinfo();
```
- to see the result, type our Ip in browser with /info.php at then end.
- it should look like some like this
![Screen Shot 2022-04-09 at 4 12 28 PM](https://user-images.githubusercontent.com/101080172/163096799-8129fadc-9fb0-44c9-87fd-d6f182fea657.png)

## Step 7: Retrieving Data from mysql database with PHP
- Connect to mysql `sudo mysql`
- create a new database ```mysql> CREATE DATABASE `example_database`;```
- create a new user name and password ```mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';```
- give the example_user user full privileges over the example_database database ```mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';```
- exit the mysql `sudo> exit`
- create a test table named todo_list 
```
  mysql> CREATE TABLE example_database.todo_list (
    -> item_id INT AUTO_INCREMENT,
    -> content VARCHAR(255),
    -> PRIMARY KEY(item_id)
    -> );
 ```
 - Insert a few rows of content in the test table. Repeat it for few times 
 - eg 
 ```
 mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO example_database.todo_list (content) VALUES ("My second important item");
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO example_database.todo_list (content) VALUES ("My third import
ant item");
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO example_database.todo_list (content) VALUES ("My and this one more thing");
Query OK, 1 row affected (0.01 sec)
```
- Confirm your data was save `mysql>  SELECT * FROM example_database.todo_list;`
- you should get this 
```
+---------+----------------------------+
| item_id | content                    |
+---------+----------------------------+
|       1 | My first important item    |
|       2 | My second important item   |
|       3 | My third important item    |
|       4 | My and this one more thing |
+---------+----------------------------+
```
- exit myql one more time `mysql> exit`
- create a PHP script that will connect to MySQL and query for your content `nano /var/www/projectLEMP/todo_list.php`
- Enter the following Script in your nano 
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
- Type your IP address follow by /todo_list.php and should get the following 
![Screen Shot 2022-04-14 at 11 08 10 PM](https://user-images.githubusercontent.com/101080172/163512333-7b3e4a7d-7fb0-4dab-b190-7330636107c6.png)
-history of my commands
```
    1  sudo apt update
    2  sudo apt install nginx
    3  curl http://localhost:80
    4  sudo apt install mysql-server
    5  sudo mysql_secure_installation
    6  mysql
    7  sudo mysql
    8  sudo apt install php-fpm php-mysql
    9  sudo mkdir /var/www/projectLEMP
   10  sudo chown -R $USER:$USER /var/www/projectLEMP
   11  sudo nano /etc/nginx/sites-available/projectLEMP
   12  sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
   13  sudo nginx -t
   14  sudo unlink /etc/nginx/sites-enabled/default
   15  sudo systemctl reload nginx
   16  sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
   17  sudo nano /var/www/projectLEMP/info.php
   18  sudo rm /var/www/projectLEMP/info.php
   19  sudo mysql
   20  mysql -u example_user -p
   21  nano /var/www/projectLEMP/todo_list.php
   ```
