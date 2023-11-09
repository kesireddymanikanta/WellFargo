# Use official maven/Java 17 image as the base image
FROM maven:3.8.4-openjdk-17-slim AS build

# Set the working directory in the image
WORKDIR /app

# Copy pom.xml and src directory (your Java application source) into the image
COPY pom.xml .
COPY . ./src

# Build a deployable .war using maven
RUN mvn clean package -Dmaven.test.skip=true

# Use Tomcat image as the base image 
FROM tomcat:10-jdk17-openjdk-slim

# Remove default Tomcat applications
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the .war file from the build image into the Tomcat webapps directory
COPY --from=build /app/target/*.war /usr/local/tomcat/webapps/ROOT.war

# Expose port 8080 for the application.
EXPOSE 8080

# Start Tomcat server
CMD ["catalina.sh", "run"]