FROM docker.io/eclipse-temurin:17

LABEL maintainer="Atul Prabhaskar"

WORKDIR /app

COPY /target/springboot-docker-demo-0.0.1-SNAPSHOT.jar /app/springboot-docker-demo.jar

# Tell Docker that the container will listen on port 8080
EXPOSE 8080

ENTRYPOINT ["java","-jar","springboot-docker-demo.jar"]