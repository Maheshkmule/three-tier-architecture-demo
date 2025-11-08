#
# Build Stage
#
FROM ubuntu:22.04 AS build

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        openjdk-8-jdk \
        maven && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /opt/shipping

COPY pom.xml .
RUN mvn dependency:resolve

COPY src ./src
RUN mvn package -DskipTests

#
# Run Stage
#
FROM eclipse-temurin:8-jdk-jammy

WORKDIR /opt/shipping

EXPOSE 8080

ENV CART_ENDPOINT=cart:8080 \
    DB_HOST=mysql

COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar

CMD ["java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar"]
