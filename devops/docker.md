# Docker

## O que é?

É uma plataforma que realiza build, execução e deploy de aplicações de forma consistente. Com o docker podemos empacotar o código com suas devidas dependencias versionadas, assim sua execução em outra máquina não terá problemas.

## Máquinas virtuais x Containers

### Máquina Virtual

São abstrações de um hardware, ou seja deve haver uma alocação de espaço e memória ao qual simularão uma máquina real.

- Há a necessidade de instalação de um sistema operacional.

### Container

São ambientes isolados para execução de uma aplicação.

- São mais leves e mais rápidos que uma máquina virtual
- São processos no sistema operacional
- Compatilham o Kernel do host

## Docker Engine

TODO: adicionar conteúdo

## Dockerfile

É um arquivo com intruções para a criação de uma Imagem. Comandos que podem ser utilizados

```dockerfile
// Especificar a imagem base
// É interessante sempre selecionar uma versão fixa
FROM node:alpine

//Especificar o diretório de trabalho
WORKDIR /app

//Copiar arquivos locais para o diretorio
COPY . .
    //Add possibilita a obtenção de um arquivo da Internet
ADD http://.../file.json .
    //Add descomprime os arquivos e move para o diretório
ADD file.zip .

//Executar comandos no sistema operacional na hora do build
RUN echo "Hello world!"

//Definir variaveis de ambiente
ENV PORT=3000

//Definir porta de trabalho
EXPOSE 80/tcp

//Especificar o usuário que irá utilizar a aplicação
USER gabriel

//Especificar o comando que deverá ser executado no inicio de um container
//Esse comando pode ser sobreescrito na hora da execução -- Há mais flexibilidade
CMD ["npm","start"] (Sempre dar preferencia)
CMD npm start  (*Inicia um novo shell)

//Para sobreescrição há a necessidade de passa a flag --entrypoint
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

Podemos adicionar um .dockerignore, ao qual irá ignorar os arquivos contidos nele.

## Image

É uma coleção de Layers compiladas através da especificação do Dockerfile, assim elas contem:

- Um sistema operacional
- Bibliotecas terceiras
- Arquivos de aplicações
- Variáveis de ambiente

## Layers

São camadas de códigos compilados ao qual compõe uma image. Docker utiliza cache para otimização da compilação das layers, para isso há a necessidade da separação das layers que terão modificações constantes, assim ficando sempre em baixo no Dockerfile.

## Comandos

<details>
<summary>Images</summary>

### Criar uma nova imagem de um Dockerfile

```
docker build -t <tag> <path>
```

- **Tag** é uma referencia adicionada para identificação da imagem. (Pode se passar uma tag de versão colocando ":" seguido da tag )
- **Path** é o caminho ao qual o Dockerfile se encontra (Normalmente se utiliza o ".").

### Listar imagens criadas

```
docker images
```

### Remover imagens

```
//Remover imagens "soltas"
docker image prune

//Remove image
docker image rm <nome ou id> ou <name:tag>
```

### Salvar e carregar imagens

```
//Salvar
docker image save -o <arquivo de saida> <image selecionada>

//Carregar
docker image load -i <arquivo>
```

### Obter imagem do Dockerhub

```
docker pull <nome_do_repo>
```

### Mudar nome de tag

```
docker image tag <nome:tag_antiga> <nome:tag_nova>
```

</details>

<details>
<summary>Containers</summary>

### Executar um container utilizando uma imagem

```
docker run [opções] <nome_da_imagem>
```

Opções:

- **-d**: Executa container em background
- **-it**: Possibilita a **interação** com o container
- **--name [Nome]**: Adiciona um nome no container
- **-p 3000:3000**: Linka porta do container com a da máquina real
- **-v [nome_do_volume]:[diretorio_no_container]**: Associa um volume ao container
- **-v $(pwd):[diretorio_no_container]**: Associa a pasta local ao volume do container

### Parar e iniciar um container

```
//Parar
docker stop <id_do_container>

//Iniciar
docker start <id_do_container>
```

## Remover container

```
docker rm -f <id do container>
docker container rm <id do container>
```

### Visualizar logs de um container

```
docker logs [opções] <id do container>
```

- **-f**: Demonstra o log de forma contínua
- **-n [numero de linhas]**: Demonstra a ultimas \*\*\* linhas
- **-t**: Demonstra o timestamp de cada log

### Listar todos os containers executando

```
docker ps [opções]
```

Opções:

- **-a**: Lista todos os containers parados

### Remover containers

```
//Remove todos os containers parados
docker container prune
```

### Executar comandos em um container em andamento

```
docker exec [opções] <container_id> [comando]
```

Opções:

- **-u <nome_do_usuario>**: logar como usuário
- **-it**: Possibilita a **interação** com o container

</details>

<details>
<summary>Volumes</summary>

### Criar um novo volume

```
docker volume create <nome-do-volume>
```

### Inspecionar um novo volume

```
docker volume inspect <nome-do-volume>
```

### Copiar arquivo de um container

```
docker cp <container_id>:<path_ao_arquivo> <path_onde_sera_salvo>
```

</details>

##Docker compose
Tem o objetivo de executar multiplos containers ao mesmo tempo.

```
//Realiza build a partir de um .docker-compose.yml
docker-compose build

//Inicia o docker compose
docker-compose up [opcoes]

opcoes:
    -d : Detach mode

//Para docker compose
docker-compose down

//Lista container da aplicação
docker-compose ps
```
