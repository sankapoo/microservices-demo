# Use a base image with Java 8 and Maven installed
FROM maven:3.8.4-openjdk-8 AS build

# Set the working directory in the container
WORKDIR /app

# Copy the Maven project descriptor files
COPY pom.xml .

# Fetch dependencies specified in pom.xml
RUN mvn dependency:go-offline

# Copy the entire project source
COPY src ./src

# Build the application using Maven
RUN mvn package -DskipTests

# Use a smaller base image for the application runtime
FROM openjdk:8-jre-slim

# Set the working directory in the container
WORKDIR /app

# Copy the built JAR file from the build stage to the runtime image
COPY --from=build /app/target/*.jar app.jar

# Expose the port that the Spring Boot application runs on
EXPOSE 8080

# Command to run the Spring Boot application when the container starts
CMD ["java", "-jar", "app.jar"]

