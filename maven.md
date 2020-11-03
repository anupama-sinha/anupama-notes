### Maven
* Powerful build tool
* Built in Java
* Automates software build tool
* Efficient transitive dependency management

### Need for Maven
* Difficult old Tasks(Downloading dependencies, adding further jars in classpath)

### Maven vs Ant
* Maven uses declarative approach(What to build & not how to built)
* Ant uses imperative approach(What to build & how to build)

### Installation
* Download [Maven](http://maven.apache.org/download.cgi) and set the environment variables as below
* M2_HOME=/usr/local/apache-maven/apache-maven-3.3.9
* M2=$M2_HOME/bin
* MAVEN_OPTS=-Xms256m -Xmx512m

### Maven Working
1. Reads pom.xml(Describes project, manages dependencies & configures plugin for building software)
2. Downloads dependencies from [central repo](https://search.maven.org/classic/#search|ga|1|centra) to local repository(C:\Users\anupama\.m2\repository)
3. Executes lifecycles, build phases & goals
4. Executes plugin
(Executes acccording to selected build profile)

### Identifier Descriptions
* modelVersion : Version of the pom model matching with maven version
* groupId : Project created by
* artifactId : Project name
* version : Semantic version of current release for project
* packaging : JAR/WAR/EAR/ZIP
* repositories : If not found in maven central repository, then url to external repo needs to be given
* properties : Value placeholders used across pom.xml as ${property-name}
* profiles : Describes the environment - dev,test,prod
* build : Describes default maven goal, compiled project directory and application final name

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.anupama.demoproject</groupId>
    <artifactId>demo-project-application</artifactId>
	<version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.3.4.RELEASE</version>
		<relativePath/>
	</parent>

    <properties>
		<java.version>1.8</java.version>
    </properties>

    <dependencies>
	    <dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-web</artifactId>
	    </dependency>
    </dependencies>

    <repositories>
        <repository>
            <id>JBoss repository</id>
            <url>http://repository.jboss.org/nexus/content/groups/public/</url>
        </repository>
    </repositories>

    <profiles>
        <profile>
            <id>production</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>org.springframework.boot</groupId>
				        <artifactId>spring-boot-maven-plugin</artifactId>
                    </plugin>
                </plugins>
            </build>
    </profile>
    
</project>
```

### Effective POM
* Resultant pom can be seen here(POM inheritance and dependencies)
> mvn help:effective-pom

### Maven Settings File
* Maven Installation Directory
> C:\Users\anupa\.m2\wrapper\dists\apache-maven-3.6.3-bin\apache-maven-3.6.3\conf\settings.xml


### Maven Build Lifecycle
* validate: validate the project is correct and all necessary information is available
* compile: compile the source code of the project
* test: test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed
* package: take the compiled code and package it in its distributable format, such as a JAR.
* integration-test: process and deploy the package if necessary into an environment where integration tests can be run
* verify: run any checks to verify the package is valid and meets quality criteria
* install: install the package into the local repository, for use as a dependency in other projects locally
* deploy: done in an integration or release environment, copies the final package to the remote repository for sharing with other developers and projects.
* clean: cleans up artifacts created by prior builds
* site: generates site documentation for this project

### Building JARs
* In progress

### References
* https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html
* http://tutorials.jenkov.com/maven/maven-tutorial.html