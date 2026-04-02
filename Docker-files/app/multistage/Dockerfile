# Stage 1: Build the application
FROM eclipse-temurin:11 AS build_image

# Install build dependencies
RUN apt-get update && apt-get install -y \
    git \
    maven

# Clone and build the project
# Note: It is often better to use 'COPY' if the code is already in your Jenkins workspace,
# but keeping 'git clone' here as per your original logic.
RUN git clone https://github.com/devopshydclub/vprofile-project.git && \
    cd vprofile-project && \
    git checkout docker && \
    mvn install

# Stage 2: Create the runtime image
FROM tomcat:9-jre11

# Clean default tomcat apps
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the artifact from the build stage
COPY --from=build_image vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh", "run"]
