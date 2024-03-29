# simple local cert 
```bash
brew install mkcert
mkcert -install
mkcert 0.0.0.0 localhost 127.0.0.1 ::1
```

# zsh "No match found"
```bash
setopt +o nomatch
setopt -o nomatch
```

# allow apps from anywhere in gatekeeper
```bash
sudo spctl --master-disable
sudo spctl --master-enable
```

# get public key
```bash
pbcopy < ~/.ssh/id_rsa.pub
```

# git revert  remote commit
```bash
git push origin +xxxxxxxx^:master
git reset HEAD^ --hard
git push origin -f
```

# callback testing
[ngrok](https://dashboard.ngrok.com/get-started)
```
./ngrok authtoken <<token>>
./ngrok http 80
```

# set time
```sh
sudo aptitude install tzdata  
sudo dpkg-reconfigure tzdata  
```

# lastest
```sh
sudo add-apt-repository ppa:pitti/postgresql
sudo add-apt-repository ppa:nginx/stable
sudo add-apt-repository ppa:ondrej/php5
```

# apt-get
```sh
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
```

# common
```sh
sudo apt-get install nload mc git curl fail2ban build-essential python-software-properties apt-show-versions zip unzip python g++ make software-properties-common
sudo apt-get install postgresql postgresql-contrib postgresql-common libpq-dev libssl-dev php5-cgi php5-mysql php5-pgsql php5-cli nginx
```

# php
```sh
sudo nano /etc/init.d/php-fastcgi
```
> insert code  

```sh
sudo chmod +x /etc/init.d/php-fastcgi
sudo /etc/init.d/php-fastcgi start
sudo update-rc.d php-fastcgi defaults
sudo /etc/init.d/nginx start
```

# erlang
```sh
sudo apt-get install erlang-base erlang-nox erlang-dev erlang-src
```

# mysql
```sh
sudo apt-get install mysql-server
mysql -u root
```
```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
exit
```
# mc
```sh
ln -s ~/.bash_profile ~/.local/share/mc/bashrc
ln -s ~/.inputrc ~/.local/share/mc/inputrc
```

# postgresql
```sh
passwd postgres ***
su postgres ***
psql
```
```sql
alter user postgres with password '***';
CREATE LANGUAGE plpgsql;
\q
```
```sh
sudo nano /etc/postgresql/8.2/main/pg_hba.conf 
```
> host all all xx.xx.xx.xx/32 trust

```sh
sudo nano /var/lib/pgsql/data/postgresql.conf 
```
> listen_addresses='*'

```sh
sudo service postgresql restart
```

# timescaledb
```ssh
sudo add-apt-repository ppa:timescale/timescaledb-ppa
sudo apt install timescaledb
sudo nano /var/lib/pgsql/data/postgresql.conf 
```
shared_preload_libraries = ‘timescaledb’
```ssh
psql
```
```sql
CREATE EXTENSION timescaledb;
SELECT create_hypertable('tablename', 'timefield');
```

# ssh
## server
```sh
mkdir -p $HOME/.ssh
chmod 0700 $HOME/.ssh
ssh-keygen -t dsa -f $HOME/.ssh/id_dsa -P ''
cat id_dsa.pub >> $HOME/.ssh/authorized_keys2
chmod 0600 $HOME/.ssh/authorized_keys2
cat id_dsa.pub >> $HOME/.ssh/authorized_keys
chmod 0600 $HOME/.ssh/authorized_keys
sudo nano /etc/ssh/sshd_config
```
> PasswordAuthentication no

```sh
sudo /etc/init.d/ssh restart
```
## client
```sh
ssh -i $HOME/.ssh/id_dsa server
nano $HOME/.ssh/config
```
> Host server.com  
>    IdentityFile ~/.ssh/id_dsa

# java
```sh
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer
java -version
```

# rust
```sh
curl https://sh.rustup.rs -sSf | sh
sudo apt-get install g++ make software-properties-common
```

# ftp
```sh
sudo aptitude install proftpd
sudo pico /etc/shells
```
> /bin/false

```sh
sudo mkdir /home/FTP-shared
sudo useradd userftp -p pass -d /home/FTP-shared -s /bin/false
sudo mkdir /home/FTP-shared/public
sudo mount -o bind /path/to/share/ /home/FTP-shared/public
sudo umount /home/FTP-shared/upload
sudo /etc/init.d/proftpd restart
```

# sftp
```sh
mkdir -p /data/sftp
chmod 701 /data
groupadd sftp_users
useradd -g sftp_users -d /upload -s /sbin/nologin sftp_user
passwd sftp_user
mkdir -p /data/sftp_user/upload
chown -R root:sftp_users /data/sftp_user
chown -R sftp_user:sftp_users /data/sftp_user/upload
nano /etc/ssh/sshd_config
```
Match Group sftp_users  
ChrootDirectory /data/%u  
ForceCommand internal-sftp -P write  
```
systemctl restart sshd
sftp sftp_user@SERVER_IP
```

# selenium
```sh
sudo apt-get install firefox xvfb
npm install selenium-webdriver
xvfb-run --server-args="-screen 0, 1366x768x24" start-selenium
```

# Redis
```sh
sudo add-apt-repository ppa:rwky/redis
sudo apt-get install redis-server
redis-cli
```
> ping

```sh
npm install -g redis-commander
redis-commander
```

# RabbitMQ
```
wget http://www.rabbitmq.com/rabbitmq-signing-key-public.asc
sudo apt-key add rabbitmq-signing-key-public.asc
sudo apt-get update
sudo apt-get install rabbitmq-server
sudo rabbitmq-plugins enable rabbitmq_management
sudo rabbitmqctl stop
sudo rabbitmq-server -detached
sudo rabbitmqctl start_app
sudo rabbitmqctl add_user login pwd
sudo rabbitmqctl set_permissions login ".*" ".*" ".*"
sudo rabbitmqctl set_user_tags login administrator
sudo rabbitmqctl set_policy HA ".*" "{\"ha-mode\": \"all\"}"
sudo nano /etc/rabbitmq/rabbitmq.config
```
```erlang
[
  {rabbit, [
  {tcp_listeners, [{"::", 5672}]},
  {default_vhost, <<"/">>},
  {default_user, <<"username">>},
  {default_pass, <<"pwd">>},
  {default_permissions, [<<".*">>, <<".*">>, <<".*">>]},
  {cluster_partition_handling, pause_minority},
  {default_user_tags, [administrator]}
  ]},
  {rabbitmq_management,[
  {listener, [{port, 15672}]},
  {load_definitions, "/etc/rabbitmq/rabbitmq_definitions.json"},
  {http_log_dir, "/var/log/rabbitmq/management_http.log"}
  ]}
].
```
```sh
sudo nano /etc/rabbitmq/rabbitmq_definitions.json
```
```erlang
{
  "vhosts":[
  {"name":"/"}
  ],
  "policies":[
  {"vhost":"/","name":"ha","pattern":"", "definition":{"ha-mode":"all","ha-sync-mode":"automatic"}}
  ]
}
```
```sh
sudo nano /etc/rabbitmq/rabbitmq-env.conf
```
> NODE_IP_ADDRESS=0.0.0.0  
> RABBITMQ_USE_LONGNAME=true  

http://stackoverflow.com/questions/31662727/define-rabbitmq-policies-in-configuration-file  
http://localhost:15672/  
post http://login:pwd@localhost:15672/api/exchanges/%2F/amq.default/publish  
```json
{"properties":{},"routing_key":"chanell_name","payload":"test","payload_encoding":"string"}
```

# RabbitMQ-cluster
open ports  
4369 (epmd)  
5672, 5671 (AMQP 0-9-1 and 1.0 without and with TLS)  
25672. This port used by Erlang distribution for inter-node and CLI tools communication and is allocated from a dynamic range (limited to a single port by default, computed as AMQP port + 20000). See networking guide for details.  
15672 (if management plugin is enabled)  
61613, 61614 (if STOMP is enabled)  
1883, 8883 (if MQTT is enabled)  
```sh
sudo epmd -names
sudo cat /proc/sys/kernel/hostname
```
http://thesoftjaguar.com/posts/2014/06/18/rabbitmq-cluster/

```sh
sudo service rabbitmq-server stop #slave
sudo cat /var/lib/rabbitmq/.erlang.cookie  #master
sudo sh -c "echo 'COOKIE_FROM_MASTER' > /var/lib/rabbitmq/.erlang.cookie"  #slave
sudo truncate -s -1 /var/lib/rabbitmq/.erlang.cookie #slave
sudo chmod 0400 /var/lib/rabbitmq/.erlang.cookie #slave
sudo chown rabbitmq:rabbitmq /var/lib/rabbitmq/.erlang.cookie #slave
sudo service rabbitmq-server start #slave
sudo rabbitmqctl stop_app #master
sudo rabbitmqctl reset #master
sudo rabbitmqctl start_app  #master
sudo service rabbitmq-server stop #slave
sudo sed -i "s/^$/xx.xx.xx.xx domain_name_from_hostname_file\n/" /etc/hosts #slave
sudo service rabbitmq-server start #slave
sudo rabbitmqctl stop_app  #slave
sudo rabbitmqctl reset  #slave
sudo rabbitmqctl join_cluster --ram rabbit@domain_name_from_hostname_file #slave
sudo rabbitmqctl start_app  #slave
```
https://thoughtsimproved.wordpress.com/2015/01/03/tech-recipe-setup-a-rabbitmq-cluster-on-ubuntu/
```sh
sudo rabbitmqctl set_policy HA ".*" "{\"ha-mode\": \"all\"}"
```

# Haproxy
```sh
sudo apt-get install haproxy
sudo nano /etc/haproxy/haproxy.cfg
```
```python
global
  log /dev/log local0
  log /dev/log local1 notice
  chroot /var/lib/haproxy
  stats socket /run/haproxy/admin.sock mode 660 level admin
  stats timeout 30s
  user haproxy
  group haproxy
  daemon

defaults
  mode tcp
  maxconn 10000
  timeout connect 5s
  timeout client 100s
  timeout server 100s
  balance roundrobin

listen rabbitmq
  bind *:5670
  server rabbitslave xx.xx.xx.xx:5672 check inter 5s rise 2 fall 3
  server rabbitmaster xx.xx.xx.xx:5672 check inter 5s rise 2 fall 3
```
```sh
sudo /etc/init.d/haproxy restart
```

# Node
```sh
curl -sL https://deb.nodesource.com/setup | sudo bash -
sudo apt-get install -y nodejs
npm install --production selenium-standalone@latest -g
npm install nodemon -g
npm install node-inspector -g
npm install node-gyp -g
npm install redis-commander -g
sudo apt-get install chromium-browser
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.25.1/install.sh | bash
nvm install 0.10
nvm use 0.10
```

# Firewall
```sh
sudo iptables-save > /root/firewall.rules
iptables-restore < /root/firewall.rules

sudo apt-get install ufw
ufw default deny
ufw default deny outgoing
sudo ufw allow ssh/tcp
sudo ufw enable
sudo ufw allow 53
sudo ufw allow 69/udp
sudo ufw allow 123/udp
sudo ufw allow 80/tcp
sudo ufw allow 8080/tcp
sudo ufw allow 8081/tcp
sudo ufw allow 3000/tcp
sudo ufw allow 443/tcp
sudo ufw allow 4444/tcp
sudo ufw allow 5432/tcp
sudo ufw allow 15672/tcp
sudo ufw allow out 53
sudo ufw allow out 25/tcp
sudo ufw allow out 587/tcp
sudo ufw allow out 123/udp
sudo ufw allow out 80/tcp
sudo ufw allow out 8080/tcp
sudo ufw allow out 443/tcp
sudo ufw allow out 3000/tcp
sudo ufw allow out 9418/tcp
sudo ufw allow from 127.0.0.1
sudo ufw allow from 78.47.92.67
sudo invoke-rc.d iptables-persistent save
```

# Mail
```sh
sudo apt-get install postfix
sudo apt-get install mailutils
sudo nano /etc/php5/conf.d/mailconfig.ini
```
> sendmail_from = "me@example.com"   
> sendmail_path = "/usr/sbin/sendmail -t -i -f me@example.com"  

```sh  
sudo dpkg-reconfigure postfix
sudo /etc/init.d/postfix reload
sudo /etc/init.d/networking restart
```

# Go
```sh
sudo add-apt-repository ppa:duh/golang
sudo apt-get update
sudo apt-get install golang
```

# Composer
```sh
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
alias composer='/usr/local/bin/composer'
composer install
```

# Bower
```sh
npm install -g bower
```

# Gulp
```sh
npm install -g gulp
```

# npm
```sh
npm install npm-check-updates -g
```

# sqllite
```sh
sudo apt-get install sqlite3 libsqlite3-dev
```

# i386
```sh
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get dist-upgrade
sudo apt-get install xxx:i386
```

# codius
```sh
sudo npm install stream-parser -g
git clone https://github.com/crypti/crypti-nacl-node
cd ./deps/
git clone https://github.com/codius/codius-sandbox.git
git fetch origin support-old-sandbox:support-old-sandbox
git checkout de639889a405dac4ffd2ed8e2eb48a5a98a0b7d2
sudo apt-get install pkg-config libseccomp libseccomp-dev libseccomp2:i386 libseccomp-dev:i386
sudo apt-get install g++-4.8 gcc-4.8 libuv-dev libcppunit-dev lib32bz2-dev libc6-dev-i386 lib32z1-dev g++-multilib
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 40 --slave /usr/bin/g++ g++
sudo update-alternatives --set gcc /usr/bin/gcc-4.8
```

# git cleanup
```sh
git checkout --orphan latest_branch
git add -A
git commit -am "commit message"
git branch -D master
git branch -m master
git push -f origin master
```

# git submodule
## create submodule
```sh
git clone git://github.com/chneukirchen/main.git
git submodule add git://github.com/chneukirchen/rack.git rack
```
## commit
```sh
git commit -a
git push
```
## checkout
```sh
git clone git://github.com/chneukirchen/main.git
git submodule init
git submodule update
```
## pull
```sh
git submodule foreach git pull origin master
```

## git apply new .gitignore
```sh
git rm -r --cached .
git add .
git commit -m "fixed untracked files"
```

## git connect via https with name-password
```sh
git remote set-url origin https://name:password@gitaddress
```

# orientdb
```sh
create database remote:http://localhost/dbname root pwd
connect remote:http://localhost/dbname root pwd
export database /orientdb/dbname.export -includeRecords=false
docker cp docker_id:/orientdb/dbname.export.gz ./
docker cp /dbname.export.gz docker_id:/orientdb/
import database /orientdb/dbname.export -preserveClusterIDs=true
```

# oracle
```sh
http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html
http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html
copy source in /opt/oracle/instantclient
export LD_LIBRARY_PATH=/opt/oracle/instantclient:$LD_LIBRARY_PATH
export OCI_LIB_DIR=/opt/oracle/instantclient
export OCI_INC_DIR=/opt/oracle/instantclient/sdk/include
ln -s libclntsh.so.12.1 libclntsh.so
apt-get install libaio1
sudo --preserve-env npm install oracledb
```
```sql
CREATE USER username IDENTIFIED BY pwd DEFAULT TABLESPACE USERS;
GRANT CONNECT TO username;
GRANT CONNECT, RESOURCE, DBA TO username;
select * from v$parameter where name='service_names' //connectString: "xx.xx.xx.xx:1521/orcl.slc.local"
grant select on sys.external_tab$ to username;
BEGIN
  FOR x IN (SELECT owner||'.'||table_name ownertab
  FROM all_tables
  WHERE owner IN ('OWNER'))
  LOOP
  EXECUTE IMMEDIATE 'GRANT ALL ON '||x.ownertab||' TO username';
  END LOOP;
END;
```

# docker
```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
apt-cache policy docker-ce
sudo apt-get install -y docker-ce
sudo systemctl status docker
sudo usermod -aG docker $(whoami)
```

# python
```sh
anaconda
conda install -c anaconda pyodbc=3.0.10
```
# ssl
```sh
openssl genrsa -des3 -out server.key 2048
openssl rsa -in server.key -out server.key.insecure
mv server.key server.key.secure
mv server.key.insecure server.key
openssl req -new -key server.key -out server.csr
sudo cp server.crt /etc/ssl/certs
sudo cp server.key /etc/ssl/private
```
# cassandra
```sh
echo "deb http://www.apache.org/dist/cassandra/debian 39x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://www.apache.org/dist/cassandra/KEYS | sudo apt-key add -
sudo apt-key adv --keyserver pool.sks-keyservers.net --recv-key A278B781FE4B2BDA
sudo apt-get update
sudo apt-get install cassandra
sudo systemctl enable cassandra
sudo service cassandra status
cqlsh localhost
sudo nodetool status
```
open ports   
7000,9042,(9160,7199)

## single node
```sh
sudo nano /etc/cassandra/cassandra.yaml
```
> listen_address: internal_ip   
> start_rpc: true   
> rpc_address: 0.0.0.0   
> broadcast_rpc_address: public_ip   
> auto_bootstrap: false   
> endpoint_snitch: GossipingPropertyFileSnitch   

## multi-node
```sh
sudo nano /etc/cassandra/cassandra.yaml
```
> listen_address: internal_ip   
> broadcast_address: public_ip   
> start_rpc: true   
> rpc_address: 0.0.0.0      
> broadcast_rpc_address: public_ip   
> seeds: "seed_public_ip_1, seed_public_ip_2"   
> auto_bootstrap: false   
> endpoint_snitch: GossipingPropertyFileSnitch   

# macosx
```sh
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

# etcd
## proxy
```sh
sudo nano /etc/default/etcd
```
ETCD_NAME="DNS_LOCAL_NAME"  
ETCD_INITIAL_CLUSTER_STATE="new"  
ETCD_ADVERTISE_CLIENT_URLS="http://0.0.0.0:2379,http://0.0.0.0:4001"  
ETCD_LISTEN_CLIENT_URLS="http://0.0.0.0:2379,http://0.0.0.0:4001"  
ETCD_INITIAL_CLUSTER="default=http://localhost:2380,default=http://localhost:7001,ip-172-31-67-54.ec2.internal=http://172.31.67.54:2380,ip-172-31-67-54.ec2.internal=http://172.31.67.54:7001,ip-172-31-69-117.ec2.internal=http://172.31.69.117:2380,ip-172-31-69-117.ec2.internal=http://172.31.69.117:7001"  
```sh
sudo systemctl restart etcd
```
## node
```sh
sudo nano /etc/default/etcd
```
ETCD_NAME="DNS_LOCAL_NAME"  
ETCD_ADVERTISE_CLIENT_URLS="http://0.0.0.0:2379,http://0.0.0.0:4001"  
ETCD_LISTEN_CLIENT_URLS="http://0.0.0.0:2379,http://0.0.0.0:4001"   
```sh
sudo systemctl restart etcd
```
