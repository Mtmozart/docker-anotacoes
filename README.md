# Meu Guia de Referência Docker

# O que é docker ?
Docker é uma plataforma de virtualização leve baseada em contêineres, que permite empacotar uma aplicação junto com todas as suas bibliotecas e dependências em uma única unidade isolada — o contêiner. Dessa forma, a aplicação passa a rodar de maneira previsível e independente do ambiente em que estiver, evitando o famoso problema do “funciona na minha máquina”. O Docker garante que a aplicação e todas as suas dependências sejam executadas de maneira consistente em qualquer sistema operacional suportado.

## Containers

Os containers no Docker são instâncias isoladas que executam uma aplicação e suas dependências como processos dentro de um único sistema operacional. Por compartilharem o kernel do host, consomem menos recursos e inicializam mais rápido do que máquinas virtuais tradicionais. Os containers oferecem isolamento em diferentes níveis (processos, sistema de arquivos e rede), garantindo eficiência, portabilidade e segurança para as aplicações.

------------------------------------------------------------------------------------------------------------------------------------------------------

Este documento serve como uma referência para consultar informações sobre a utilização do Docker. Abaixo estão listados diversos comandos e conceitos importantes relacionados a containers, imagens, volumes, redes e muito mais.

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


## 🐳 Imagens

As imagens do Docker são **pacotes leves e independentes** que contêm todos os elementos necessários para executar uma aplicação — desde o sistema de base, as dependências e as bibliotecas até o próprio código da aplicação.

### 🔍 Comandos úteis para trabalhar com imagens:

* **Listar todas as imagens locais:**
  `docker images`

* **Exibir detalhes de uma imagem:**
  `docker inspect <image_id|nome:tag>`

* **Exibir o histórico de camadas de uma imagem:**
  `docker history <image_id|nome:tag>`

* **Remover uma imagem específica:**
  `docker rmi <image_id|nome:tag>`

* **Remover todas as imagens locais:**
  `docker rmi $(docker images -aq)`

* **Baixar uma imagem do Docker Hub:**
  `docker pull <nome:tag>`

* **Construir uma imagem a partir de um Dockerfile:**
  `docker build -t <nome_da_imagem> .`


### 💾 Volumes e Persistência de Dados

Para persistir dados em containers, o Docker oferece diferentes opções, sendo as mais comuns:

#### **Volumes**

* Criar:
  `docker volume create <nome_volume>`

* Listar todos:
  `docker volume ls`

* Utilização em um container:
  `docker run -it -v <nome_volume>:/app ubuntu bash`

* Inspecionar detalhes:
  `docker volume inspect <nome_volume>`

* Remover um volume específico:
  `docker volume rm <nome_volume>`

* Limpar volumes não usados:
  `docker volume prune`

#### **Bind Mounts**

Usado para montar uma pasta do host no container:
`docker run -it -v /caminho/local:/app ubuntu bash`

#### **tmpfs**

Usado para armazenar dados temporários apenas na memória RAM do host (voláteis):
`docker run -it --tmpfs /app:rw,size=64m ubuntu bash`

---

### 🌐 Redes

O Docker cria redes para permitir comunicação entre containers:

* Listar todas as redes:
  `docker network ls`

* Criar uma nova rede (usando o driver padrão `bridge`):
  `docker network create <nome_rede>`

* Inspecionar detalhes de uma rede:
  `docker network inspect <nome_rede>`

* Conectar um container a uma rede existente:
  `docker network connect <nome_rede> <container>`

* Desconectar um container de uma rede:
  `docker network disconnect <nome_rede> <container>`

---

docker compose up
Inicia os serviços definidos no arquivo docker-compose.yml. Se as imagens não existirem, elas serão construídas ou baixadas.

docker compose up -d
Inicia os serviços em modo “detached” (em segundo plano).

docker compose down
Para e remove os containers, redes e volumes criados pelo Compose.

docker compose ps
Lista os containers e o status dos serviços em execução.

docker compose logs
Exibe os logs dos serviços.

docker compose logs -f
Exibe os logs em tempo real (follow).

docker compose build
Constrói ou reconstrói as imagens dos serviços definidos.

docker compose stop
Para os containers em execução, mas não remove.

docker compose start
Inicia containers parados.

docker compose restart
Reinicia os containers.

docker compose exec <serviço> <comando>
Executa um comando em um container em execução (exemplo: abrir um shell).

docker compose run <serviço> <comando>
Executa um comando temporário em um novo container do serviço.

docker compose config
Exibe a configuração completa, após processamento e merges, para validação.

docker compose version
Mostra a versão do Docker Compose.

## 🐳 O que é um Dockerfile?

Um **Dockerfile** é um arquivo de texto que contém um conjunto de instruções que o Docker usa para criar uma imagem. Essas instruções definem como o ambiente da aplicação será configurado, quais dependências serão instaladas, quais arquivos serão copiados, quais portas serão expostas e qual comando será executado para iniciar a aplicação.

---

## 🛠️ Exemplo de Dockerfile para uma aplicação NestJS

```dockerfile
# 1. Imagem base leve do Node.js
FROM node:18-alpine

# 2. Definir o diretório de trabalho dentro do container
WORKDIR /app

# 3. Copiar arquivos de dependências para aproveitar cache
COPY package*.json ./

# 4. Instalar dependências da aplicação
RUN npm install

# 5. Copiar o restante dos arquivos do projeto
COPY . .

# 6. Compilar a aplicação NestJS
RUN npm run build

# 7. Expor a porta padrão que o NestJS utiliza
EXPOSE 3000

# 8. Comando para rodar a aplicação compilada
CMD ["node", "dist/main.js"]
```

---

## ⚙️ Passo a passo do funcionamento

1. **Imagem base**: começa com uma imagem oficial do Node.js, versão leve (`alpine`) para otimizar espaço.
2. **Diretório de trabalho**: define a pasta `/app` dentro do container onde os arquivos serão copiados e os comandos serão executados.
3. **Gerenciamento de dependências**: copia os arquivos `package.json` e `package-lock.json` primeiro, instala as dependências com `npm install` — isso possibilita o cache do Docker para acelerar builds futuros.
4. **Copiar o código fonte**: adiciona o restante dos arquivos para dentro do container.
5. **Build da aplicação**: executa o script `npm run build`, que transpila o código TypeScript para JavaScript, gerando a pasta `dist`.
6. **Exposição da porta**: torna a porta 3000 disponível para comunicação externa.
7. **Inicialização da aplicação**: define o comando padrão para rodar a aplicação NestJS compilada.


