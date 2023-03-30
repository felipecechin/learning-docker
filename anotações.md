#Docker

## Conceitos

Cada arquivo `Dockerfile` é um projeto, e cada projeto é uma imagem, que pode ser rodada em um container. A imagem tem instruções para criar um container, e o container é a instância da imagem.

Cada instrução do arquivo representa uma camada (layer). Quando uma camada é atualizada, o Docker cria uma nova camada, e não altera a anterior. Isso é bom para performance, pois se uma camada for alterada, não é necessário recriar todas as camadas anteriores.

O comando que precisar inserir ID de imagem ou container, pode ser as 3 primeiras letras do ID.

É possível termos múltiplos containers rodando a mesma imagem.

## Comandos

##Gerais

### Ajuda em qualquer comando do docker:

- `docker <comando> --help`

### Remover imagens, containers e volumes não utilizados:

- `docker system prune`

##Imagens

###Gerar imagem baseada no `Dockerfile`:

- `docker build .`

##### Nomear imagem direto no build:

- `docker build -t <nome> .`

##### Nomear imagem com tag:

- `docker build -t <nome>:<tag> .`

O `.` no final do comando significa que o `Dockerfile` está na pasta atual.

###Listar imagens:

- `docker image ls`
- `docker images`

###Nomear imagens:

- `docker tag <ID> <nome>`

###Inserir tags nas imagens:

- `docker tag <ID> <nome>:<tag>`

###Remover imagem:

- `docker rmi <ID | name>`

##### Para forçar remoção, caso a imagem esteja sendo usada por algum container:

- `docker rmi -f <ID | name>`

###### Com tag:

- `docker rmi <nome>:<tag>`

##Containers

###Roda container baseada no ID da imagem:

- `docker run <ID>`

##### Rodando em background e expondo porta:

- `docker run -d -p 8080:80 <ID>`

Nesse caso, a porta 8080 do host será exposta para a porta 80 do container.

##### Rodando no modo interativo:

- `docker run -it <ID>`

##### Nomear container:

- `docker run --name <nome> <ID>`

##### Remover container após parar:

- `docker run --rm <ID>`

###Iniciar/parar container:

- `docker start <ID | name>`
- `docker stop <ID | name>`

##### Iniciar no modo interativo:

- `docker start -i <ID | name>`

###Listar containers:

##### Listar container ativos:

- `docker ps`

##### Listar container ativos e inativos:

- `docker ps -a`

### Remover container

- `docker rm <ID | name>`

### Copiar arquivos do container para a máquina:

- `docker cp <nome-container>:<path> <path-maquina>`

### Informações de containers

##### Informações de processamento do container:

- `docker top <nome-container>`
- `docker stats <nome-container>` ou somente `docker stats`

##### Informações de configuração do container:

- `docker inspect <nome-container>`

## Volumes

### Listar volumes

- `docker volume ls`

### Criar volume

- `docker volume create <nome-volume>`

### Informações de volume

- `docker volume inspect <nome-volume>`

### Remover volume

- `docker volume rm <nome-volume>`

### Remover volume não utilizado

- `docker volume prune`

### Criar container com volume

##### Volume anônimo

- `docker run -d -p 8080:80 -v /data <ID-da-imagem>`

##### Volume nomeado

- `docker run -d -p 8080:80 -v <nome-volume>:/data <ID-da-imagem>`

##### Bind mount (mapeamento de diretório)

- `docker run -d -p 8080:80 -v <path-maquina>:<path-container> <ID-da-imagem>`

## Redes

### Listar redes

- `docker network ls`

### Criar rede

- `docker network create <nome-rede>`

### Remover rede

- `docker network rm <nome-rede>`

### Remover rede não utilizada

- `docker network prune`

## Docker Swarm

```
Comandos no Docker Labs:
CTRL C no Play Docker: CTRL + Insert
CTRL V no Play Docker: CTRL + Shift + V
```

### Iniciar swarm e definir o nó como manager

- `docker swarm init`

##### Iniciar swarm e definir o nó como manager e especificar o IP do nó

- `docker swarm init --advertise-addr <IP>`

### Adicionar um nó ao swarm

- `docker swarm join --token <token> <IP>:<PORTA>`

### Sair do swarm

- `docker swarm leave -f`

-f é para forçar a saída do swarm, mesmo que seja o último nó.

### Nodes ativos

- `docker node ls`

### Subindo um serviço

- `docker service create --name <nome-serviço> -p <porta>:<porta> <nome-imagem>`

##### Com replicas

- `docker service create --name <nome-serviço> -p <porta>:<porta> --replicas <número-replicas> <nome-imagem>`

### Checar token do Swarm

- `docker swarm join-token worker`

### Fazer worker sair do swarm

- `docker swarm leave`

### Remover node

- `docker node rm <ID>`

### Inspecionar serviço

- `docker service inspect <nome-serviço>`

### Ver quais containers um serviço está rodando

- `docker service ps <nome-serviço>`

### Rodando Compose com Swarm

- `docker stack deploy -c <nome-arquivo-compose> <nome-stack>`

### Fazer serviço não receber mais tasks

- `docker node update --availability drain <ID>`

### Atualizar imagem de um serviço

- `docker service update --image <nome-imagem> <nome-serviço>`
