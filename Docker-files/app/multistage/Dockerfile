#Build Image
FROM openjdk:11 AS BUILD_IMAGE
RUN apt update && apt install maven -y 

#Copy source code
RUN git clone https://github.com/devopshydclub/vprofile-project.git
#mvn will generate artifact
RUN cd vprofile-project && git checkout docker && mvn install

#Second Image
FROM tomcat:9-jre11
#delete the default image
RUN rm -rf /usr/local/tomcat/webapps/*
#Copy our build_image and move it to the location
COPY --from=BUILD_IMAGE vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
#when we type docker run it will run catalina.sh that and host our artifact
CMD ["catalina.sh", "run"]
