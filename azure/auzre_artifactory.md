## Azure Artifactory(Library) CI
* Commit the code in Repository
* Create a new Pipeline for CI with Project Name,Repository Name & Default Branch Name with below agent jobs
1. Maven Authenticate : Enter Feed Name(Artifactory name where this is to be linked) with enabled control option
2. Command Line(Git Config) : git config --global user.email "email-id", git config --global user.name "Anupama Sinha", git checkout main
3. Maven(Building Jar) : Goal as package with pom.xml mentioned
4. Command Line(Publishing Jar) : mvn -B release:clean release:prepare release:perform -Dusername=project-name -Dpassword=${azure-pipeline-variable} (Library project-env-variables will have key-value azure-pipeline-variable

* This will be accessible in Azure Artifacts, Connect to Feeds & all versions are visible
* It can be used by copying Maven settings.xml in .m2 folder locally with Personal Access Token for the username

settings.xml(In .m2 folder)

```xml
<settings ...>
  <servers>
    <server>
      <id>repo-name</id>
      <username>project-name</username>
      <password>personal-access-token</password>
    </server>
  </servers>
</settings>
```
pom.xml(Project)

```xml
<repositories>
  <repository>
    <id>artifactory-name</id>
    <url>artifactory-url</url>
    <releases>
      <enabled>true</enabled>
    </releases>
    <snapshots>
      <enabled>true</enabled>
    </snapshots>
  </repository>
</repositories>
```

* POM Semantic Version can be incremented during build. Git Checkout needed as Azure CI checks for latest commit instead of Detached state. Username & password needed as Maven release commits incremented version number
