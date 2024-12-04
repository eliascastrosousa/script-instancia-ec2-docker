### Script para colocar ao criar uma instancia ec2, visando deixar a maquina preparada para receber arquivos docker e docker compose, este script sera rodado ao ser gerada a instancia.

#### Amazon Linux 
```
sudo yum update -y && sudo yum install docker -y && sudo usermod -a -G docker ec2-user && sudo service docker start && newgrp docker && sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose && sudo chmod +x /usr/local/bin/docker-compose

sudo yum update -y

sudo rm /usr/local/bin/docker-compose
sudo ln -s /Applications/Docker.app/Contents/Resources/cli-plugins/docker-compose /usr/local/bin/docker-compose

```

### Depois de criado a instancia, enviar os arquivos para a criação do container com as imagens do projeto 

```
sudo scp -i chave.pem -r docker-compose.yml nginx env ec2-user@IP_DA_INSTANCIA:/home/ec2-user
```

#### Amazon Ubuntu 

```
  # Add Docker's official GPG key:
  sudo apt-get update
  sudo apt-get install ca-certificates curl
  sudo install -m 0755 -d /etc/apt/keyrings
  sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
  sudo chmod a+r /etc/apt/keyrings/docker.asc

  # Add the repository to Apt sources:
  echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  sudo apt-get update
```

```
  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

```
 sudo apt-get update
```

```
 docker compose version
```


