# AWS
# 2 Tier app deployment on AWS

#### Ec2 instance for our nodeapp
#### Ec2 instance for our DB
#### Networking on AWS with VPC, Subnets, NACLs, Security groups


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
### Create reverse proxy so we can load our app without the 3000 port
- `sudo nano /etc/nginx/sites-available/default`
- change the reverse proxy setting to redirect the traffic from 3000 to default port 80
```
server {
    listen 80;

    server_name _;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}


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

 ### Create env variable in our app ec2 to connect to db

```
 sudo echo "export DB_HOST=mongodb://172.31.45.161:27017/posts" >> ~/.bashrc
```
- Source the .bashrc file

- `source ~/.bashrc `

### What is a VPC
- VPC virtual private cloud to define and control virtual network
- VPC enables you to launch AWS resources into a virtual network that you've. This virtual network closely resembles a traditionl network that you'd operate in your data centre
- It allows us to EC2 instances to communicate with each other, we can also create multiple subnets within out VPC
- it benefits us with scalability of the infrastruture of AWS

### Internet gateway
- Inetnet is the point which allowed us to connect to Internet
- A gateway that you attach to your VPC to enable communication between resources in your VPC and the internet

### What is a Subnet
- Network inside the VPC, they make network more sufficient 
- A range of IP addressess in your VPC 
- A subnet could have multiple ec2 instances

### Route Table
- Set of rules, called routes
- Route tables are used to determine where external network traffic is directed 

### NACLS
- NACLS are an added layer of defence they work at the network level
- NACLs are stateless, you have to have rules to allow the request to come in and to allow the response to go back out
- Public NACLs Inbound Rules
- 100 Allows inbound HTTP 80 traffic from any IPv4 address.
- 110 Allows inbound SSH 22 traffic from your network over the internet
- 120 allows inbount return traffic from hosts on the internet that are responding to requests originating in the subnet - TCP 1024-65535

#### NACLs outbound Rules
- 100 to allow port 80
- 110 we need the cirdr block and allow 27017 for outbound access to our Mongo DB server in private subnet
- 120 to allow short lived ports between 1024-65535

### Private NACLs Inbound Rules
- Inbound rules




- outbount rules


### What is Security Group
- Security groups work as a firewall on the instance level
- They are attached to the VPC and subnet
- They have inbound and outbound traffic rules defined 
- Security groups are stateful, if you allowed inbount rule that will automatically be allowed outbound
 
 ### What are the Ephemeral ports
- They are shortly lived ports, they are automatically allocated based on the request/demand
- Allows outbound responses to clients on the internet
- they range from 1024-65535

#### Useful resources links
https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html
https://docs.aws.amazon.com/vpc/latest/userguide/how-it-works.html

### What is S3
 - uses cases
 - who is using S3 in the industry 
 - setting up s3, dependencies
 - configure AWSCLI
 - how can we get the authentication done to talk with S3
 - AWS access and secret 
 - we will apply crud
 - S3: you will have a backup available to apply CRUD in the console of AWS

 **we need running EC2 to ssh into the instance and AWS access and secret**


 - S3 is a Simple storage service provided by AWS
 - It is used to store and retrieve any amount of data, at anytime, from around the world
 - We can also host our static website on S3

 - Create a bucket from AWSCLIT
 - upload data
 - download data
 - delete data
 - permissons of the bucket 

 - In order have AWSCLI we need to install the required dependencies
 - Python
 - pip
 - Configure the AWSCLI with AWS keys to authenticate the access from our machine to S3

 -  `aws s3 sync s3://eng84shahrukhs3 README.md` to download the data from our S3 bucket

AWS CLI reference:
```
aws s3

aws s3 cp to copy data from your instance to S3 bucket

aws s3 mb make bucket

aws s3 mv move

aws s3 ls list files

aws s3 rb remove bucket

aws s3 rm remove file/data

aws s3 sync download data
```