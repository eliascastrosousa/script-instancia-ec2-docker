# Criando Instancia utilizando o Free Tier da AWS

### Imagem de máquina da Amazon
Estarei utilizando a maquina Linux Ubuntu, pela familiaridade, mas também outras opções no free tier, uma das mais usadas é a Amazon linux, que utiliza o gerenciador de pacotes yum. 

![Imagem da maquina e nome da instancia](https://github.com/user-attachments/assets/1d40ad89-e248-49f3-9bc1-c46ffcd681cd)

### Tipo de Instancia
Para o tipo de instancia estou utilizando o t2.micro, que também se qualifica para o nivel gratuito.

![Tipo de Instancia](https://github.com/user-attachments/assets/6b206e79-c416-48bb-baee-7f55cd580c71)

### Par de chaves (Login)
O par de chaves é importante para uma conexão segura atraves do SSH e envio de arquivos com SCP. Para o par de chaves estou criando uma especifica para este laboratorio chamada chave-sgb, do tipo ED25519 no formato .pem pois irei utilizar atravez de ssh no terminal. 

![Par de chaves](https://github.com/user-attachments/assets/52746f56-6c35-4414-b175-cf28972332e1)


### Configurações de rede 
Esta parte é importante pois nela que você ira configurar o acesso externo a sua aplicação para que seja possível acessar o servidor via SSH e via browser. neste caso habilitaremos as opções  Permitir tráfego de SSH, Permitir tráfego de HTTP e Tráfego de HTTPS

![Par de chaves](https://github.com/user-attachments/assets/d04ea058-5631-41c1-a523-270e6b178cd0)

### Detalhes avançados
Por ultimo a ultima configuração é em Dados de usuario, colar o script que inicializara a maquina com as seguintes configurações
- Instação do Docker
- Instalação do docker compose
- Atualização dos pacotes
- configurar permissão de Root no docker

Amazon Linux Ubuntu:

```
#!/bin/bash
sudo apt-get -y update

# INSTALANDO DOCKER
sudo apt-get install -y ca-certificates curl gnupg lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --batch --yes --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get -y update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io

# Iniciando e habilitando docker para já iniciar ativo
sudo systemctl enable docker.service
sudo systemctl enable containerd.service

# Instalando docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Configurando permissão no docker para não ter que ficar usando root
sudo usermod -aG docker ubuntu
newgrp docker

```
### Amazon Linux Ubuntu

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

#### Depois de criado a instancia, enviar os arquivos para a criação do container com as imagens do projeto 

```
  sudo scp -i chave.pem -r docker-compose.yml nginx env ec2-user@IP_DA_INSTANCIA:/home/ec2-user
```

#### Depois de instalado, 

```
  sudo scp -i chave.pem -r docker-compose.yml nginx env ec2-user@IP_DA_INSTANCIA:/home/ec2-user
```

#### rodar docker compose para testar: 

```
 sudo docker compose up --build
```

