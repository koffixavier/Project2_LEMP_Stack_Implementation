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
