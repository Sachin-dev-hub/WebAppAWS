the sample form fill web applicaton pushed to github

> docker file
FROM tomcat:latest

# Optional: copy back default Tomcat apps
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps

# Copy all WAR files into Tomcat webapps
COPY webapps.war /usr/local/tomcat/webapps/
> Jenkins
- git url, /main directory
- goal and option: clean install
- post build action: ssh server, select docker user credentials which we created in docker and added in the jenkins, transfer set: webapp/target/*.war, remove prefix: webapp/target, Remote directory: //opt//docker,
- Exec command:
cd /opt/docker && \
docker rm -f $(docker ps -qa)
docker rmi $(docker images -q)
docker build -t tomimage:v1 . && \
docker run -d --name tomc1 -p 8086:8080 tomimage:v1
docker cp /opt/docker/*.war tomc1:/usr/local/tomcat/webapps

