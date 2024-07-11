# Banco de dados PostgresSQL 

docker-compose.yml

```
services:
  postgres:
    container_name: api
    image: postgres
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=senha1234
      - POSTGRES_DB=api
      - POSTGRES_MAX_CONNECTIONS=100
      - POSTGRES_IDLE_IN_TRANSACTION_SESSION_TIMEOUT=60000
      - POSTGRES_TCP_KEEPALIVES_IDLE=600
      - POSTGRES_TCP_KEEPALIVES_INTERVAL=60
      - POSTGRES_TCP_KEEPALIVES_COUNT=10
    ports:
      - 5432:5432
    volumes:
      - data:/var/lib/postgresql/data
    restart: always
volumes:
      data:
```

# Para subir o container digite
```
docker-compose up -d
```
# Para Derrubar o container digite
```
docker-compose down
```

### OBS.. Você tem que estar no diretorio do arquivo docker-compose.yml


# Dockerfile rodar aplicação backend springboot

# JDK 21
```
FROM ubuntu:latest AS build

RUN apt-get update
RUN apt-get install openjdk-21-jdk -y
COPY . .

RUN apt-get install maven -y
RUN mvn clean install

FROM openjdk:21-jdk-slim 
EXPOSE 8080

COPY --from=build /target/backend-0.0.1.jar app.jar

ENTRYPOINT [ "java", "-jar", "app.jar" ]
```
# JDK 17
```
FROM ubuntu:latest AS build

RUN apt-get update
RUN apt-get install openjdk-17-jdk -y
COPY . .

RUN apt-get install maven -y
RUN mvn clean install

FROM openjdk:17-jdk-slim 
EXPOSE 8080

COPY --from=build /target/nome_da_api-0.0.1.jar app.jar

ENTRYPOINT [ "java", "-jar", "app.jar" ]
```
### OBS.. Devemos trocar o nome_da_api com base no nome que está no POM.XML e tbm a versão você tera algo assim no seu POM.XML apague o SNAPSHOT 0.0.1-SNAPSHOT  nome_da_api-0.0.1, e pronto só rodar esse docker file.
# Comando para criar um build Dockerfile
```
docker build -t nome_da_imagem .
```
# Comando para iniciar um Dockerfile
```
docker run -dti -p 8080:8080
```
# Comando para parar um Dockerfile
```
docker stop nome_do_contêiner
```
# Comando sh para execurar um docker-compose e um Dockerfile 
build_and_run.sh
```
#!/bin/bash

# Iniciar os serviços do docker-compose
docker-compose up -d

# Construir a imagem da API com Dockerfile
docker build -t nome_da_api .

# Executar a API
docker run -dti -p 8080:8080

```
# Comando pra exutar o sh
```
./build_and_run.sh
```
Obs.. terminal git ou linux 
