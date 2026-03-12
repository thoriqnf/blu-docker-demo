# Write your Dockerfile here!
# Refer to the README.md for instructions.
# ===========================================
# Multi-stage Dockerfile for Todo API
# ===========================================
# Stage 1: Build the application with Maven
# Stage 2: Run with a lightweight JRE image
# ===========================================

# ---------- Stage 1: Build ----------
FROM maven:3.9-eclipse-temurin-21-alpine AS builder

WORKDIR /build

# Copy dependency descriptor first (for better layer caching)
COPY pom.xml .

# Download dependencies (cached unless pom.xml changes)
RUN mvn dependency:go-offline -B

# Copy source code
COPY src ./src

# Build the application (skip tests for faster builds)
RUN mvn clean package -DskipTests -B

# ---------- Stage 2: Runtime ----------
FROM eclipse-temurin:21-jre-alpine

# Add labels for better container management
LABEL maintainer="devops-training"
LABEL description="Todo API - Spring Boot Application"
LABEL version="1.0.0"

# Create a non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

# Copy the built JAR from the builder stage
COPY --from=builder /build/target/notes-app.jar app.jar

# Change ownership to non-root user
RUN chown -R appuser:appgroup /app

# Switch to non-root user
USER appuser

# Expose the application port
EXPOSE 8080

# Health check (use BusyBox wget-compatible flags)
HEALTHCHECK --interval=30s --timeout=10s --start-period=40s --retries=3 \
    CMD wget -q --spider http://localhost:8081/actuator/health || exit 1

# Run the application; read DB password from Docker secret if present
ENTRYPOINT ["sh", "-lc", "if [ -f /run/secrets/db_password ]; then export DB_PASSWORD=$(cat /run/secrets/db_password); fi; exec /opt/java/openjdk/bin/java -jar app.jar"]
