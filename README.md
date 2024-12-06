# Criando Instancia utilizando o Free Tier da AWS + Docker pré instalado

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
Após finalizar este passo a passo, a maquina será inicializada e estara disponivel em seu console EC2.



Utilize este [passo a passo](https://github.com/eliascastrosousa/registrar-dominio-instancia-aws) para registrar um IP Fixo para sua nova instancia e registrar um dominio. 


### Acessando a maquina atravez do SSH

```
  sudo ssh -i chave-sgb.pem USUARIO@IP_DA_INSTANCIA ou DOMINIO 
```

Se caso a instancia iniciar sem o docker compose, instale com o seguinte comando: 

```
  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Atualize novamente os pacotes

```
 sudo apt-get update
```

Verifique as instalações

```
docker --version && git --version && docker-compose --version && docker compose version
```
![image](https://github.com/user-attachments/assets/f5a5f8c0-395e-4cd0-88c9-ea28a49ee77e)


### Osquestrando o container

Envie os arquivos necessarios para subir seu container. 

Se caso você não tiver gerado a imagem e subido no Docker Hub, este [passo a passo](https://github.com/eliascastrosousa/gerar-imagem-docker-hub) pode te ajudar. 
Arquivos a serem enviados: 
- diretorio env (com os arquivos app.env e mysql.env)
- docker-compose.yml
- diretorio nginx (onde estará o arquivo nginx.conf para o proxy reverso)
  
( nos arquivos deste repositorio tem exemplos dos arquivos a serem utilizados. )

```
  sudo scp -i chave-sgb.pem -r env nginx docker-compose.yml  USUARIO@IP_DA_INSTANCIA:/home/usuario
```

Rodando docker compose para construir os constainers e testar a aplicação 

```terminal
 sudo docker compose up --build
```

