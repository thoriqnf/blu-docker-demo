# Multi stage dockerfile for todo API JAVA
# Stage 1: build the app with maven
# Stage 2: run with JRE image already slim

FROM maven:3.9-eclipse-temurin-21-alpine AS builder

WORKDIR /build

COPY pom.xml .

RUN mvn dependency:go-offline -B

COPY src ./src

RUN mvn clean package -D skipTests -B

# Stage 2

FROM eclipse-temurin:21-jre-alpine

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

# copy the built jat from the builder stage

COPY --from=builder /build/target/todo-app.jar app.jar

# non root user ownership
RUN chown -R appuser:appgroup /app

USER appuser

EXPOSE 7070

HEALTHCHECK --interval=30s --timeout=30s --start-period=40s --retries=3 CMD [ "executable" ]

ENTRYPOINT [ "java", "-jar", "app.jar" ]