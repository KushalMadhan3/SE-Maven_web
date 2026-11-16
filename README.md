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


# deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 4
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
        - name: web-container
          image: myapp:latest
          ports:
            - containerPort: 8080


# FLASK DOCKERfile
FROM python:3.11-slim
WORKDIR /app
COPY . .
RUN pip install flask
EXPOSE 5000
CMD ["python", "app.py"]



EXPOSE 8080


# Run Tomcat in foreground

CMD ["catalina.sh", "run"]
