### Script para colocar ao criar uma instancia ec2, visando deixar a maquina preparada para receber arquivos docker e docker compose, este script sera rodado ao ser gerada a instancia.

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

### Depois de criado a instancia, enviar os arquivos para a criação do container com as imagens do projeto 

```
sudo scp -i chave.pem -r docker-compose.yml nginx env ec2-user@IP_DA_INSTANCIA:/home/ec2-user
```
