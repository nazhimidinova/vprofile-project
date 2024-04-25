#This specifies the base image as openjdk:11, pull it from Docker Hub and names it BUILD_IMAGE. It updates the package list and installs Maven.
FROM openjdk:11 AS BUILD_IMAGE
RUN apt update && apt install maven -y 

#This clones the source code of the project from the specified GitHub repository.
RUN git clone https://github.com/devopshydclub/vprofile-project.git

#This changes the directory to the cloned repository, checks out the docker branch, and builds the project using Maven. The mvn install command compiles the source code, runs tests, and packages the application into a .war file.
##mvn will generate artifact
RUN cd vprofile-project && git checkout docker && mvn install

#Second Image
##This specifies the base image as tomcat:9-jre11, which includes Tomcat 9 with Java 11 runtime environment.
FROM tomcat:9-jre11
#delete the default image
RUN rm -rf /usr/local/tomcat/webapps/*
#Copy our build_image and move it to the location
COPY --from=BUILD_IMAGE vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
#when we type docker run it will run catalina.sh that and host our artifact
#This sets the default command to execute when the container starts. It runs the catalina.sh script with the run argument, which starts the Tomcat server and hosts the deployed application.
CMD ["catalina.sh", "run"]

##this Dockerfile first builds the Java web application using Maven 
## in one stage, and then deploys the built artifact to a Tomcat server in another stage. 
##This allows for separation of concerns and ensures that the final image only contains the 
##necessary runtime dependencies for serving the application. When the container is run,
## it will start Tomcat and serve the deployed application on port 8080.