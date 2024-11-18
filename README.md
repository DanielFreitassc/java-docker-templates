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

# JDK 21 temurin
```
# Etapa 1: Build com Maven
FROM maven:3.9.9-eclipse-temurin-21-alpine AS build
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

# Etapa 2: Execução com OpenJDK 21
FROM azul/zulu-openjdk-alpine:21
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar  
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"] 
```
