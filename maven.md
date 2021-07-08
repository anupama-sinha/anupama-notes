### Maven
* Powerful build tool built in Java 
* Automates software build tool
* Efficient transitive dependency management
* Builds, Publishes & deploys projects at once

### Need for Maven
* Difficult old Tasks(Downloading dependencies, adding further jars in classpath)

### Maven vs Ant
* Maven uses declarative approach(What to build & not how to build)
* Ant uses imperative approach(What to build & how to build)

### Installation
* Download [Maven](http://maven.apache.org/download.cgi) and set the environment variables as below
* M2_HOME=/usr/local/apache-maven/apache-maven-3.3.9
* M2=$M2_HOME/bin : Helps in accessing CMD line of Maven
* MAVEN_OPTS=-Xms256m -Xmx512m
* Path=%M2%

### JDK Mapping for Multi JDK environment(Optional)
In mvn.cmd file, please make below changes
@REM===START VALIDATION
set JAVA_HOME=C:\Program Files\Java\jdk1.8.0_261\jre

### Maven Wrapper for MVN Version(Optional)
> mvn -N io.takari:maven:wrapper -Dmaven=3.3.3

### Maven Working
1. Reads pom.xml(Describes project, manages dependencies & configures plugin for building software)
2. Downloads dependencies from [central repo](https://search.maven.org/classic/#search|ga|1|centra) to local repository(C:\Users\anupama\.m2\repository)
3. Executes lifecycles, build phases & goals
4. Executes plugin
(Executes acccording to selected build profile)

### Maven Repository
* Directories of packaged JAR files that contain metadata(POM) based on which dependencies are downloaded
* Types
    1. Local : Local PC
    2. Remote : Any server
    3. Central : Internet(Maven Community)

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

### Maven Build Lifecycle : Sequence of build phases which in turn has sequence of goals
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

### Build Profiles
* Configurational values required to build project using different configurations for corresponding builds for different environments

### Build Plugins
* Group of goals which may or may not be of same phase
* Performs specific goals

### Maven Architecture
* Maven -> Reads pom.xml -> Connects with Maven Repository -> Does the task

### Build Phases
* Compile -> Test-Compile -> Test -> Package -> Integration-test -> Verify -> Install -> Deploy

### Commands
> mvn --version
> mvn archetype:generate  
> mvn dependency:tree

### Maven Release(mvn release)
* release:clean -> Performs clean up after release preparation
* release:prepare -> Prepares release in SCM. Phases to ensure POM ready for release & creates a tag in SVN which can be sued by release:perform to make a release

```
<scm>
	<developerConnection>scm:svn:URL</deveoperConne
</scm>
```

* release:rollback -> Rollback to previous release
* release:perform -> Performs a release from SCM by downloading tagged version from SCM
* release:prepare-with-pom -> Prepares from previous Build

### Maven Deploy
* In progress

### Building JARs
* In progress

### Maven Templates
* There are various templates which can be used for maven. Most common one is org.apache.maven.archetypes : maven-archetype-quickstart
* Similarly, for other languages, different ones can be used. It minimizes boiler plate coding

### References
* https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html
* http://tutorials.jenkov.com/maven/maven-tutorial.html
