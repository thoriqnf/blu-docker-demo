# Write your Dockerfile here!
# Refer to the README.md for instructions.

#Build
FROM maven:3.9.13-eclipse-temurin-21-alpine AS builder

WORKDIR /build

COPY pom.xml .

RUN mvn dependency:go-offline -B

COPY src ./src

RUN mvn clean package -DskipTests -B

#Runtime
FROM eclipse-temurin:21-jre-alpine

LABEL maintainer="kevin.kusuma"
LABEL description="notes-app - Spring Boot Application"
LABEL version="1.0.0"

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

COPY --from=builder /build/target/notes-app.jar app.jar

# Change ownership to non-root user
RUN chown -R appuser:appgroup /app

# Switch to non-root user
USER appuser

# Expose the application port
EXPOSE 8081

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=40s --retries=3 \
    CMD wget --quiet --tries=1 --spider http://localhost:8081/actuator/health || exit 1

# Run the application
ENTRYPOINT ["java", "-jar", "app.jar"]