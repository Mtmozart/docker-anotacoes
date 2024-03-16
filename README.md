# Meu Guia de Referência Docker

Este documento serve como uma referência para consultar informações sobre a utilização do Docker. Abaixo estão listados diversos comandos e conceitos importantes relacionados a containers, imagens, volumes, redes e muito mais.

## Containers

Os containers no Docker funcionam como processos dentro de um único sistema, consumindo menos recursos. Eles são isolados em diferentes níveis, garantindo segurança e eficiência.

- **Namespace**: Capacidade de se isolar em determinados níveis, incluindo PID, NET, IPC, MNT e UTS.

### Comandos Úteis:

- `docker ps`: Lista os containers em execução.
- `docker container ls -a`: Lista todos os containers.
- `docker run image 1d`: Inicia um container com duração de um dia.
- `docker stop <container_id>`: Para um container em execução.
- `docker start <container_id>`: Reinicia um container.
- `docker ps -s`: Visualiza o tamanho dos containers.
- `docker exec -it <container_id> bash`: Acessa um container de forma interativa.
- `docker pause <container_id>`: Pausa a execução de um container.
- `docker unpause <container_id>`: Tira um container do estado de pausa.
- `docker inspect <container_id>`: Exibe informações detalhadas sobre um container.
- `docker rm <container_id>`: Remove um container.
- `docker rm <container_id> --force`: Remove um container forçadamente.
- `docker run ubuntu bash`: Executa um comando diretamente em um container.
- `docker container rm $(docker container ls -aq)`: Remove todos os containers.

## Mapeamento de Portas

O Docker permite mapear portas entre o host e o container, facilitando o acesso aos serviços executados nos containers.

- `docker run -d -P dockersamples/static-site`: Inicia um container com portas aleatórias.
- `docker port <container_id>`: Verifica as portas mapeadas para um container.
- `docker run -d -p 8080:80 dockersamples/static-site`: Define manualmente a porta de acesso ao container.

## Imagens

As imagens do Docker são pacotes leves e independentes que contêm todos os elementos necessários para executar uma aplicação.

- **Comandos**:
  - `docker images`: Lista as imagens disponíveis.
  - `docker inspect <image_id>`: Exibe detalhes sobre uma imagem.
  - `docker history <image_id>`: Mostra as camadas de uma imagem.
  - `docker rmi $(docker image ls -aq)`: Remove todas as imagens.

## Persistência de Dados

Para persistir dados em containers, o Docker oferece diferentes opções, incluindo bind mounts, volumes e tmpfs.

- **Volumes**:
  - Criar: `docker volume create <nome_volume>`
  - Visualizar: `docker volume ls`
  - Utilizar: `docker run -it -v <nome_volume>:/app ubuntu bash`
  - Inspect: `docker volume inspect <nome_volume>`

## Redes

O Docker permite criar redes para que os containers possam se comunicar entre si.

- **Comandos**:
  - `docker network ls`: Lista as redes disponíveis.
  - `docker network create --driver bridge <nome_rede>`: Cria uma nova rede.

## Docker Compose

O Docker Compose é uma ferramenta para definir e executar aplicativos Docker multi-container. Abaixo estão alguns comandos úteis relacionados ao Docker Compose.

- **Comandos**:
  - `docker-compose up -d`: Inicia os serviços definidos no arquivo docker-compose.yml em modo detached (em segundo plano).
  - `docker-compose down`: Para todos os serviços definidos no arquivo docker-compose.yml e remove os containers associados.
  - `docker-compose logs`: Exibe os logs dos serviços definidos no arquivo docker-compose.yml.
  - `docker-compose ps`: Lista os serviços e seus status definidos no arquivo docker-compose.yml.

Este guia é apenas uma introdução ao Docker e seus principais conceitos e comandos. Para mais informações e exemplos, consulte a [documentação oficial do Docker](https://docs.docker.com/).
