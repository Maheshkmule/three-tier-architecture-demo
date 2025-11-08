# -------------------------------
# Stage 1: Build the application
# -------------------------------
FROM maven:3.9.6-eclipse-temurin-17 AS build

WORKDIR /app

# Copy pom.xml and download dependencies first (for caching)
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy source code and build the jar
COPY src ./src
RUN mvn clean package -DskipTests

# -------------------------------
# Stage 2: Create the runtime image
# -------------------------------
FROM eclipse-temurin:8-jdk-jammy

WORKDIR /app

# Expose application port
EXPOSE 8080

# Copy only the built JAR from the build stage
COPY --from=build /app/target/*.jar app.jar

# Environment variables (can override in Kubernetes manifest)
ENV JAVA_OPTS="-Xms256m -Xmx512m"

# Run the application
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]
