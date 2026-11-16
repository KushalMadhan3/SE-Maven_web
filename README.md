MAVEN_WEB

TRIGGER

MAVENWEB-DOCKER(FREESTYLE):
     copy target\*.war app.war
(
echo FROM tomcat:9-jdk17
echo COPY app.war /usr/local/tomcat/webapps/app.war
) > Dockerfile
docker build -t mavenweb:latest .
docker rm -f mavenweb 2>nul
docker run -d --name mavenweb -p 8082:8080 mavenweb:latest



DOCKERfile:
FROM tomcat:9.0-jdk17-temurin


# Optional: Clean existing default ROOT app

RUN rm -rf /usr/local/tomcat/webapps/ROOT


# Copy your packaged Maven WAR file as the default root application

COPY target/mavenweb.war /usr/local/tomcat/webapps/ROOT.war


EXPOSE 8080


# Run Tomcat in foreground

CMD ["catalina.sh", "run"]
