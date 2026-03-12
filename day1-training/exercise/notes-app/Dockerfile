# Write your Dockerfile here!
# Refer to the README.md for instructions.

FROM maven:3.9-eclipse-temurin-21-alpine AS builder

WORKDIR /build

COPY /pom.xml .

RUN mvn dependency:go-offline -B

COPY /src ./src

RUN mvn clean package -DskipTests -B

FROM eclipse-temurin:21-jre-alpine

RUN addgroup -S appnotes && adduser -S appuser -G appnotes

WORKDIR /app

COPY --from=builder /build/target/notes-app.jar app.jar

RUN chown -R appuser:appnotes /app

EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]
