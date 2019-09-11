# Oracle Docker Setup

```sh
# Step for Squadron Node:

curl -fsSL https://get.docker.com/ | sh
# yum-config-manager enable *addons*
# yum install docker-engine
useradd oracle
usermod -a -G docker oracle
sed -i '/.*ExecStart.*/c\ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --insecure-registry pbhagade-oracle.openstacklocal:5000' /usr/lib/systemd/system/docker.service
systemctl daemon-reload
systemctl restart docker
systemctl enable docker
# docker info
echo "172.26.88.239     pbhagade-oracle.openstacklocal"   >> /etc/hosts
docker pull pbhagade-oracle.openstacklocal:5000/oracle/database:12.2.0.1-ee   ### 6 mins to pull
cd ~
mkdir oradata
chmod a+w oradata
docker run --name oracle-eee -p 1521:1521 -v /home/oracle/oradata:/opt/oradata pbhagade-oracle.openstacklocal:5000/oracle/database:12.2.0.1-ee  ##  12 mins
###  docker run will keep the oracle running, if nodes goes down. We can use below cmds to stop and start.
docker stop oracle-eee
docker start oracle-eee  ### it takes 1 minutes to startup.

# Resetting the database admin accounts passwords
docker exec oracle-ee ./setPassword.sh PASSWORD
```
