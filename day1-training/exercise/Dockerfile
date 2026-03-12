# Write your Dockerfile here!
# Refer to the README.md for instructions.

# Build
FROM maven:3.9-eclipse-temurin-21-alpine AS builder
WORKDIR /build
COPY notes-app/pom.xml .
RUN mvn dependency:go-offline -B
COPY notes-app/src ./src
RUN mvn clean package -DskipTests -B

# Runtime
FROM eclipse-temurin:21-jre-alpine

LABEL maintainer="notes-app-exercise"
LABEL description="Notes APP"
LABEL version="1.0.0"

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app
COPY --from=builder /build/target/notes-app.jar app.jar

RUN chown -R appuser:appgroup /app
USER appuser

EXPOSE 8081
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
    CMD wget --quiet --tries=1 --spider http://localhost:8081/actuator/health || exit 1

ENTRYPOINT ["java", "-jar", "app.jar"]