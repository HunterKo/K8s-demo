# Use maven to compile the java application.
FROM maven:3.8.6-openjdk-8-slim AS build-env

RUN mkdir -p /opt/app/build
WORKDIR /opt/app/build

COPY . ./
RUN mvn -Dmaven.test.skip=true package

# Build runtime image.
FROM openjdk:8-jre-alpine

WORKDIR /opt/app
COPY --from=build-env /opt/app/build/target/*.jar /opt/app/app.jar

ENTRYPOINT ["java","-jar","app.jar"]