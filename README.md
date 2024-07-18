
## 
```
https://successlifemantra.com:8090/
pass+fobsam#1Aa
```

# 1 nvm installation
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
`````
```
source ~/.bashrc
```
```
nvm ls-remote
```
```
nvm install 18.4.0
```
```
node -v
```
## 2 GIT installation
```
sudo apt install git
```
```
git config --global user.name "venkat"
```
```
git config --global user.email "venkat.r@mindvisiontechnologies.com"
```
## 3 PM2 installation
```
npm install pm2 -g
```
```
pm2 -v
```
## 4 NGINX installation
```
sudo apt install nginx
```
```
sudo ufw enable
```
```
sudo ufw status
```
```
sudo ufw allow ssh
```
```
sudo ufw allow http
```
```
sudo ufw allow https
```
```
systemctl status nginx
```
## 5 SSH connection - github
```
ssh-keygen -t rsa
```
enter passpharse and note that carefully
```
cat ~/.ssh/id_rsa.pub
```
copy the output string start as `ssh-rsa` - `root@[name]` to github.com -> settings -> ACCESS -> ssh and cpg keys -> new ssh key -> give title and paste the string and add
```
ssh -T git@github.com
```
type `yes` and enter passpharse
```
eval "$(ssh-agent -s)"
```
```
ssh-add /root/.ssh/id_rsa
```
enter the passpharse
## 6 clone repo from github
```
git clone git@github.com:MindVisionTechnologies/webhook.git
```
!!! use `ssh` method not `https` and
```
cd webhook
```
```
rm -r node_modules
```
```
npm i
```
```
node .
```
check if the server is running properly and if ok press `CTRL+C` to exit
## 7 add servers to PM2
```
cd webhook
```
```
pm2 start index.js --name webhook
```
!! check the index file may be `index.js` or `server.js`
```
pm2 ls
```
to list all pm2 processes
## 8 link PM2 process to NGINX
```
sudo nano /etc/nginx/sites-available/webhook.mindvisiontechnologies.com
```
```
  server{
    index index.js;
    server_name webhook.mindvisiontechnologies.com;
    location / {
        proxy_pass http://localhost:9999;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
    }
```
replace `server_name` with your `servername`as domain name and `proxy_pass` as localhost that rubs with pm2
```
nginx -t
```
it check the file configuration and return success message
```
sudo systemctl reload nginx
```
reload  nginx and check the domain
## 9 install certbot
```
sudo apt install certbot
```
```
sudo apt-get update
```
```
sudo apt-get install python3-certbot-nginx
```
```
sudo certbot --nginx
```
it will show the domains with and enter the number of domain to generate ssl certificate
# disable unattended upgrades
Certainly! Here's the arranged code for disabling unattended upgrades in Ubuntu:
1. Open the `50unattended-upgrades` file with `nano`:
   ```
   sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
   ```
2. Inside the file, you'll find lines like these for configuring automatic updates:
   ```
   Unattended-Upgrade::Allowed-Origins {
           "${distro_id}:${distro_codename}-security";
           // "${distro_id}:${distro_codename}-updates";
           // "${distro_id}:${distro_codename}-proposed";
           // "${distro_id}:${distro_codename}-backports";
   };
   ```
 ## 3. Disable the Unattended-Upgrades Service**: You can also disable the service
```
sudo systemctl disable unattended-upgrades
```
# Disable certbot auto renew
```
sudo systemctl disable certbot.timer
```
```
sudo systemctl stop certbot.timer
```
# Remove snap service from linux
```
sudo snap remove lxd && sudo snap remove core22  && sudo apt purge snapd && sudo rm -rf /var/cache/snapd/ && sudo rm -rf ~/snap
```
# Allocate swap memory
change the 1G depend on the server ram if 512 change the 1G to 512MB or more
```
sudo fallocate -l 1G /swapfile
```
```
sudo chmod 600 /swapfile
```
```
sudo mkswap /swapfile
```
```
sudo swapon /swapfile
```
```
echo '/swapfile swap swap defaults 0 0' | sudo tee -a /etc/fstab
```
```
sudo swapon --show
```
### New swap memory Cmd
```
 sudo su
```
```
sudo dd if=/dev/zero of=/swapfile bs=128M count=16
```
```
free -m
```
```
cd / 
```
```
ls   { check there is a swapfile}
```
```
ls -la { check there is a  -rw-r--r--}
```
```
sudo chmod 600  /swapfile
```
```
sudo mkswap  /swapfile
```
```
sudo swapon /swapfile
```
```
sudo swapon -s
```
```
free -m
```
```
htop
``` 

# rabbitmq
```
sudo apt-get update
```
```
sudo apt-get install -y erlang
```
```
echo 'deb http://www.rabbitmq.com/debian/ testing main' |
sudo tee /etc/apt/sources.list.d/rabbitmq.list
```
```
wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc |
sudo apt-key add -
```
```
sudo apt-get update
```
```
sudo apt-get install rabbitmq-server
```
```
sudo systemctl enable rabbitmq-server
```
```
sudo systemctl start rabbitmq-server
```
```
sudo systemctl status rabbitmq-server
```
```
sudo rabbitmq-plugins enable rabbitmq_management
```
#### allow firewall
```
sudo ufw allow 5672/tcp
```
```
sudo ufw allow 15672/tcp
```
```
sudo ufw status
```
```
sudo systemctl restart rabbitmq-server
```
create user
```
sudo rabbitmqctl add_user <myuser>  <mypassword>
```
```
sudo rabbitmqctl set_user_tags <myuser> administrator
```
```
sudo rabbitmqctl set_permissions -p / <myuser> ".*" ".*" ".*"
```
acces with
http://[your-server-ip]:15672
# Mongodb server
```
sudo apt-get install gnupg curl
```
```
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```
```
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```
```
sudo apt-get update
```
```
sudo apt-get install -y mongodb-org
```
```
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-database hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-mongosh hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
```
```
sudo systemctl start mongod
```
```
sudo systemctl daemon-reload
```
```
sudo systemctl status mongod
```
```
sudo systemctl enable mongod
```
```
sudo ufw allow 27017
```
```
sudo ufw allow 27017/tcp
```
make public connections
```
mongosh --host localhost
```
it will return to shell, local host working
```
mongosh --host <ipaddress>
```
it will refuse the connection
```
nano /etc/mongod.conf
```
add your ip address in bindIp next to the 127.0.0.1 with ','
```
bindIp 127.0.0.1,137.184.227.198
```
```
sudo systemctl restart mongod
```
```
sudo systemctl status mongod
```
now connect with ip `mongosh --host <ipaddress>` it will work
# redis server
```
sudo apt update
```
```
sudo apt install redis-server
```
```
sudo systemctl start redis-server
```
```
sudo systemctl enable redis-server
```
```
sudo systemctl status redis-server
```
```
sudo ufw allow 6379
```
```
sudo ufw allow 6379/tcp
```
make public connections
```
redis-cli --host localhost --port 6379
```
it will access shell result in OK
```
redis-cli -h 142.93.221.104 -p 6379
```
it will refuse the connection
```
nano /etc/redis/redis.conf
```
redirect to this file and change the bind directive
```
bind 127.0.0.1 142.93.221.104
```
add you ip address next to the 127.0.0.1 with single white space
```
sudo systemctl restart redis-server
```
```
redis-cli -h 142.93.221.104 -p 6379
```
it will connect to the shell
