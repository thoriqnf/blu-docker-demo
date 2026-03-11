# Write your Dockerfile here!
# Refer to the README.md for instructions.
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

WORKDIR /app

# Copy the built JAR from the builder stage
COPY --from=builder /build/target/notes-app.jar notes-app.jar

# Expose the application port
EXPOSE 8081

# Run the application
ENTRYPOINT ["java", "-jar", "notes-app.jar"]

# docker network create my-custom-network
# # Start the Server
# docker run -d --name my-server --network my-custom-network nginx

# # Start the Client and PING by name
# docker run --rm --network my-custom-network alpine ping -c 3 my-server