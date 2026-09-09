换源
sudo vim /etc/docker/daemon.json
{
   "registry-mirrors": [
       "https://mirror.ccs.tencentyun.com"
   ]
}
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo docker info

授予权限
sudo groupadd docker
sudo usermod -aG docker ubuntu
newgrp docker