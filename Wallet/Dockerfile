# ------------ BUILD STAGE ------------
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Copy Maven wrapper + pom
COPY pom.xml .
COPY mvnw .
COPY .mvn .mvn
RUN mvn -q dependency:go-offline

# Copy source code
COPY src ./src

# Build jar
RUN mvn clean package -DskipTests


# ------------ RUNTIME STAGE ------------
FROM eclipse-temurin:17-jdk
WORKDIR /app

# Copy built jar from build stage
COPY --from=build /app/target/*.jar app.jar

# Wallet service port
EXPOSE 8082

# Activate prod profile + allow Render to set dynamic PORT
ENTRYPOINT ["sh", "-c", "java -jar app.jar --spring.profiles.active=prod --server.port=${PORT:-8082}"]
