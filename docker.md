### Docker
* Application: Artifact(JAR+WAR) + Environemnt
* Docker App -> JVM -> OS
* Docker CLI(Commands) -> Docker Daemon -> OS
* Create Image -> Docker Image -> OS(Own)
> java -jar spring-rest-application.jar -> Makes it run in JVM
* [OS + Install Java]  + JAR file  -> Run JAR file
   Support Team          Dev Team 
* Containers : Images which are running
* Containerzation is better than virtualization

### Docker Engine
* Helps in creating executable images

### Docker Client
* CLI to build, publish, pull, run images
* Invokes Docker Daemon REST API endpoint for doing the work

### Docker Deamon
* Contains images & containers

### Docker Objects
* Images, Containers/Services, Network, Volume

### Docker Registry
* Stores images(Eg. Docker hub, AWS container registry, google container registry etc.)

### Dockerfile
* Commands to build image

### Docker Storage
* Maintains single copy of shared data
* Volume(Dir mounted) : Common across container to use the shared data

### Docker Compose
* Multiple containers/services running together

### Legacy Way to run an application in OS
1. Install Windows 10/Centos(Centos OS Size- Virtual OS- LINUX variant of Java)
2. Install Java 1.8
> yum install java(No installer in Linux, so run yum command)
* 
> brew install java(In Mac)

3. Copy JAR file to root path or any specific path
4. Run JAR(Write in quotes)
java -jar spring-rest-application.jar

### Docker way to run the application
1. Create Docker file
* Example 1 : Centos OS
```yaml
FROM centos [Centos empty OS]
RUN yum install -y java
VOLUME /tmp [Containers are stateless and data selected each time. So can use the dir specified in volume across containers and when started again or newly]
COPY target/abc.jar abc.jar
ENTRYPOINT["java","-jar","/spring-rest-application.jar"]
```
* Example 2 : Alpine OS
```yaml
FROM alpine:3.2 
RUN apk --update add openjdk8-jre   [apk command instead of yum]
COPY target/abc.jar abc.jar
ENTRYPOINT["java","-jar","/spring-rest-application.jar"]
```
* Example 3 : Install Java with Installed OS
```yaml
FROM openjdk8-jre-slim
WORKDIR /home [Default target path for all throughput. Prefixed in each path]
COPY target/abc.jar abc.jar
EXPOSE 8080
ENTRYPOINT["java","-jar","/spring-rest-application.jar"]
```
* Example 4 : catalina base installs tomcat in /usr/local/tomcat
```yaml
FROM tomcat:latest
add target/sample.war /usr/local/tomcat/webapps/
EXPOSE 8080
cmd["catalina.sh","run"]
```

* Create image(-t : Specifies name, . : dockerfile path in current directory, If name not mentioned, then alphanumeric)
> docker build -t anu-application:1.0.

* Check images
> docker images

* Check containers
> docker ps

* Stop containers(Returns container id if stopped successfully)
> docker stop container-id

* Start container
> docker start container-id

* Stop all containers running
> docker ps -a

* Remove all stopped containers
> docker container prune

* Run the images. This runs in centos OS. So, cant be accessed from our OS
> docker run anu-application

* Accessing in localhost[5000 is outside port accessible from localhost]
> docker run -p 5000:8080 anu-application

* Force delete image
> docker image rm -f anu-application

* Debug Container(-it: internative, else detached, exec: run inside docker container, bash: cmd prompt, sh: cmd prompt, ctrl+d: come out of container)
> docker exec -it container-name bash

### Docker Commands
* Check version 
docker -v 
