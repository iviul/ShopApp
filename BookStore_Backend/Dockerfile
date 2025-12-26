# Build stage
FROM maven:3.8.7-eclipse-temurin-17 AS build

WORKDIR /app

# Copy pom.xml and download dependencies (cached layer)
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code and build (skip tests - they need database)
COPY src ./src
RUN mvn clean package -DskipTests -B

# Runtime stage
FROM eclipse-temurin:17-jre

WORKDIR /app

EXPOSE 8080

# Copy the built artifact from build stage
COPY --from=build /app/target/bookstore_backend-0.0.1-SNAPSHOT.jar /app/bookstore_backend.jar

ENTRYPOINT ["java", "-jar", "bookstore_backend.jar"]