# ------------ BUILD STAGE ------------
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Copy maven wrapper and pom first for caching
COPY pom.xml .
COPY mvnw .
COPY .mvn .mvn
RUN mvn -q dependency:go-offline

# Copy source and build
COPY src ./src
RUN mvn clean package -DskipTests

# ------------ RUNTIME STAGE ------------
FROM eclipse-temurin:17-jdk
WORKDIR /app

# Copy jar from build stage
COPY --from=build /app/target/*.jar app.jar

# Expose service port (container-level)
EXPOSE 8086

# Use prod profile and support Render dynamic PORT
ENTRYPOINT ["sh", "-c", "java -jar app.jar --spring.profiles.active=prod --server.port=${PORT:-8086}"]
