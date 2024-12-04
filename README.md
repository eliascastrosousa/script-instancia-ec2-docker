### Script para colocar na area de Dados do usuário (opcional) localizada em Detalhes avançados ao criar uma Instancia Ec2 dentro do Console AWS, com este script a maquina ira ser gerada já instalado o Docker e Docker-compose

```
sudo yum update -y 
sudo yum install docker -y 
sudo yum install git -y
sudo usermod -a -G docker ec2-user 
sudo service docker start && newgrp docker 
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose 
sudo chmod +x /usr/local/bin/docker-compose
sudo yum update -y 
sudo rm /usr/local/bin/docker-compose
sudo ln -s /Applications/Docker.app/Contents/Resources/cli-plugins/docker-compose /usr/local/bin/docker-compose

```
