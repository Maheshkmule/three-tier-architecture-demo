#
# Build Stage
#
FROM ubuntu:22.04 AS build

# Install Java 8, Maven, and Certificates
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        openjdk-8-jdk \
        maven \
        ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /opt/shipping

# Copy project files
COPY pom.xml /opt/shipping/
RUN mvn dependency:resolve

COPY src /opt/shipping/src/
RUN mvn package

#
# Run Stage
#
FROM eclipse-temurin:8-jdk-jammy

EXPOSE 8080
WORKDIR /opt/shipping

# Environment variables for microservice communication
ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql

# Copy the JAR from the build stage
COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar

# Start the application
CMD ["java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar"]
