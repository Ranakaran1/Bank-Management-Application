# ------------ BUILD STAGE ------------
FROM maven:3.9.6-eclipse-temurin-21 AS build
WORKDIR /app

# Copy pom and maven wrapper for caching
COPY pom.xml .
COPY mvnw .
COPY .mvn .mvn
RUN mvn -q dependency:go-offline

# Copy source and build
COPY src ./src
RUN mvn clean package -DskipTests

# ------------ RUNTIME STAGE ------------
FROM eclipse-temurin:21-jdk
WORKDIR /app

# Copy built jar
COPY --from=build /app/target/*.jar app.jar

# Expose container port for transaction service
EXPOSE 8083

# Run with prod profile and allow Render to override PORT
ENTRYPOINT ["sh", "-c", "java -jar app.jar --spring.profiles.active=prod --server.port=${PORT:-8083}"]
