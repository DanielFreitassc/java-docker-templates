# Banco de dados  imgens 

```yml
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

# Dockerfile rodar aplicação backend springboot
# JDK 22
```dockerfile
# Use a base image that includes OpenJDK 22
FROM eclipse-temurin:22-jdk-alpine

# Install Maven
RUN apk add --no-cache maven

# Set the working directory
WORKDIR /app

# Copy the Maven project files
COPY . .

# Run Maven to build the project
RUN mvn clean install

EXPOSE 8080

RUN cp target/*.jar app.jar

ENTRYPOINT [ "java", "-jar", "app.jar" ]
```
# JDK 21
```dockerfile
FROM ubuntu:latest AS build

RUN apt-get update
RUN apt-get install openjdk-21-jdk -y
COPY . .

RUN apt-get install maven -y
RUN mvn clean install

FROM openjdk:21-jdk-slim 
EXPOSE 8080

COPY --from=build /target/*.jar app.jar

ENTRYPOINT [ "java", "-jar", "app.jar" ]
```
# JDK 17
```dockerfile
FROM ubuntu:latest AS build

RUN apt-get update
RUN apt-get install openjdk-17-jdk -y
COPY . .

RUN apt-get install maven -y
RUN mvn clean install

FROM openjdk:17-jdk-slim 
EXPOSE 8080

COPY --from=build /target/*.jar app.jar

ENTRYPOINT [ "java", "-jar", "app.jar" ]
```
