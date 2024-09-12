FROM openjdk:11 AS BUILD_IMAGE
RUN apt update && apt install maven -y
RUN git clone https://github.com/YaminiObinendi/vprofile-project1-.git
WORKDIR vprofile-project1-
RUN git checkout docker && mvn install


FROM tomcat:9-jre11
RUN  rm -rf /usr/local/tomcat/webapps/ROOT
COPY --from=BUILD_IMAGE vprofile-project1-/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh","run"]
