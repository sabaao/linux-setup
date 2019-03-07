# How to install nexus on Centos7

#install openjdk

sudo yum -y install java-1.8.0-openjdk

#install wget

sudo yum -y install wget setup perl

#change to /opt

cd /opt

#get nexus

sudo yum update

sudo wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz

#unzip

sudo tar -xzvf latest-unix.tar.gz -C /opt

#create link

sudo ln -s nexus-* nexus

#create nexus user

sudo useradd nexus -M -s /sbin/nologin

#authorization

sudo chown -R nexus:nexus /opt/nexus

sudo chown -R nexus:nexus /opt/sonatype-work/

#edit /opt/nexus/bin/nexus.rc

sudo vi /opt/nexus/bin/nexus.rc

#remove remark of run_as_user

run_as_user="nexus"

#setting service

sudo vi /etc/systemd/system/nexus.service

#add content to nexus.service
[Unit]
Description=nexus service
After=network.target
[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
User=nexus
Restart=on-abort
[Install]
WantedBy=multi-user.target

#setup service
sudo systemctl daemon-reload
sudo systemctl enable nexus
sudo systemctl start nexus

#check service
sudo systemctl status nexus

#test nexus is alive
curl http://0.0.0.0:8081

#change port to 80
cd /opt/nexus/etc
sudo vi nexus-default.properties

#change application-port to 80
application-port=80

#restart service
sudo systemctl restart nexus

## Reference 
* http://blog.sina.com.cn/s/blog_704836f40102x06j.html
* https://edxi.github.io/2018/04/16/NexusYum/
