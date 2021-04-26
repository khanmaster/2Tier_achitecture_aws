# AWS
# 2 Tier app deployment on AWS
# Ec2 instance for our nodeapp
# Ec2 instance for our DB



- Launch an ec2 instance with correct version of ubuntu
- ssh into the instance
- update 
- upgrade
- install nginx 
- access nignx page with public IP
- share the IP in the chat


### Install nodejs with correct version on EC2 instance for our app
```
!/bin/bash

# Update the sources list
sudo apt-get update -y

# upgrade any packages available
sudo apt-get upgrade -y

# install nginx
sudo apt-get install nginx -y

# install git
sudo apt-get install git -y

# install nodejs
sudo apt-get install python-software-properties
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install nodejs -y

# install pm2
sudo npm install pm2 -g
```

### install mongodb on EC2 instance for DB

```
wget -qO - https://www.mongodb.org/static/pgp/server-3.2.asc | sudo apt-key add -

 echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

 sudo apt-get update

 sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20



sudo mkdir -p /data/db

 sudo chown -R mongodb:mongodb /var/lib/mongodb

 sudo sed -i 's/127.0.0.1/0.0.0.0/g' /etc/mongod.conf

 sudo systemctl enable mongod

 sudo service mongod start
 ```

 