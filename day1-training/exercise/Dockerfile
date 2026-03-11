# Write your Dockerfile here!
# Refer to the README.md for instructions.

FROM maven:3.9-eclipse-temurin-21-alpine AS builder
WORKDIR /build
COPY ./notes-app/pom.xml .
RUN mvn dependency:go-offline -B
COPY ./notes-app/src ./src
RUN mvn clean package -DskipTests -B


FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY --from=builder /build/target/notes-app.jar app.jar
EXPOSE 8081
# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=40s --retries=3 \
    CMD wget --quiet --tries=1 --spider http://localhost:8080/actuator/health || exit 1
ENTRYPOINT ["java", "-jar", "app.jar"]