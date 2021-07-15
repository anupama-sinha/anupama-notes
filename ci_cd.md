## Git Branching Policies
* Commit intial code in main/master
* Create a sub-branch develop
* Create all sub-stories in develop subbranches(Feature Branch)
* Once coding is complete, commit the code in Feature Branch and raise Pull Request(PR) from feature to develop

## Automated Pull Request(PR) Review Setup(Branch Policies for develop)
* Minimum number of reviewers set
* If required, requestors allowed to approve their own changes
* Check for Comment Resolution kept as Required
* Limit merge types can be disabled. If need be, it can be kept as any type(Basic Merge-Stores commits for all, Squash Merge-Gives only one commit and keeps develop commits clean, Rebase and fast forward, Rebase with merge commit). Mostly developers prefer basic merge or squash merge
* Automatic trigger for Build Validation kept for prior validation of CI Build stability. It makes a temporary copy of develop and appends the feature commit and build the code
* Optional reviewers can be kept
* Eg. Branching Policies in develop in dev.azure.com

## CI Build for PR for Feature Branch
* Source and destination branches chosen
* Maven Authenticate used for common library JARs(Kept as feeds in artifacts)
* Maven : Used for goal package, Publishing JUnit Test Results to Azure Pipelines, Test result files for Surefire XMLs given
* Then it is saved
* Once queued, it starts agent job where a new OS instance(Eg. Ubuntu) given for each agent job run
* Once job run succeeds, feature branch gets merged to develop branch
* Post develop merge, CI/CD Build initiates
* Eg. Pipeline Creation(CI for PR Build)

## CI Build for Develop Branch
* Builds code, publishes test coverage report, builds Docker Image & pushes Image to Container Registry(Eg ACR in Azure or Hub Docker for Public Images,etc)
* Source and destination branches chosen
* Maven Authenticate used for common library JARs(Kept as feeds in artifacts)
* Maven : Used for goal package, Publishing JUnit Test Results to Azure Pipelines, Test result files for Surefire XMLs given
* Docker Image build : ACR type chosen, Azure Subscription(Service Connection for dev & portal of Azure for Azure Resource Manager type), ACR details given, Build An image, Dockerfile path given for base directory, Using default build context, Image name given as project-name:$(Build.buildId)
* Docker Image push : ACR type, Service Connection for Azure, ACR details, Push an Image action, Image name give as above,Qualify Image name
* Then it can be deployed with Kubectl deploy(CD Build) but that should be extracted as separate one as best practice
* Instead, copy the Docker folder(File : deployment.yaml) to target folder${Build.ArtifactStagingDirectory} which is unique for Agent job
* Then publish the Pipeline Artifact(${Build.ArtifactStagingDirectory}) to publish location(Azure Pipelines)
 
## CD Build for Profiled Deployment
* Step starts as soon as Docker Image published to Pipeline. It picks Docker Image and publishes to AKS
* Create artifact in Pipeline Release job : Provide project name, source build pipeline for latest version. Enable the Continuous Deployment trigger
* Create stage for Environment Deployment with Enabled Artifact Filters pointing to CICD Job. Deployment tasks as below
1. Replace Token(For build.buildid in deployment.yaml as Azure publishes common tag) : Mention Root Directory of CICD Job, Target as yaml files
2. Kubectl(For kubectl apply -f configmap.yaml/secrets.yaml/deployment.yaml : Separate 3 tasks) : Mention Service Connection(Azure Resource Manager for linking dev & portal), Resource groups, AKS & Command as apply
* Then it is saved & queued
* Once job run succeeds, Docker Image is deployed in AKS

## CI for Common Library
* Source & destination branches chosen for project
* Maven Authenticate for choosing Azure feed
* GIT Config : Mentions GIT Username , email and checks out specific branch(Eg. main)
* MAven : Building JAR using goal package
* CLI to Publish JAR(ran in backfround to avoid locks) to Artifact Feed using below command and Personal Access Token(Generated for Azure)
> mvn -B release:clean release:prepare release:perform -DreleaseVersion=1.0 -DdevelopmentVersion=1.0.1-SNAPSHOT -Dusername=<MY-USERNAME> -Dpassword=<MY-PAT>

## Dockerfile

```text
FROM openjdk:8-jdk-alpine
COPY target/spring-project.jar spring-project.jar
EXPOSE 8085
ENTRYPOINT ["java","-jar","spring-project.jar"]
```
 
## ConfigMaps

```yaml
kind: ConfigmMap
apiVersion: v1
metadata:
 name: project-configmap-name
data:
 spring.profiles.active: dev
```
 
## Secrets
* Secrets values can be seen as below
> kubectl edit secrets project-secret-name
```yaml
kind: Secret
apiVersion: v1
type: Opaque
metadata:
 name: project-secret-name
data:
 DB_PASSWORD: base64-encoded-password
```
 
## Deployments
```yaml
apiVerersion: apps/v1
kind: Deployment
metadata:
 labels:
  app: spring-project-name
 name: spring-project-name
spec:
 replicas: 1
 selector:
  matchLabels:
   app:spring-project-name
 template:
  metadata:
   labels:
     app: spring-project-name
  spec:
     continaers:
     - image: acr-url/spring-project-name:#{Build.BuildId}#
       name: spring-project-name
       imagePullPolicy : Always
       ports:
         -containerPort: 8085
       envFrom:
         - configMapRef:
             name: spring-project-name
       env:
         -name : db.pwd
          valueFrom:
            secretKeyRef:
              name: project-secret-name
              key: DB_PWD
 ---
 apiVersion: v1
 kind: Service
 metadata:
   name: spring-project-name
   annotations:
     service.beta.kubernetes.io/azure-load-balancer-resource-group: resource-group-name
 spec:
  type: LoadBalancer
  selector:
   app: spring-project-name
 ports:
  -protocol: TCP
   port: 80
   targetPort: 8080
```

## Logging to AKS Cluster
* az login (Then login with email ID)
* az aks get-credentials --resource-group <<resource-grp-name>> --name <<AKS Cluster Name>> (This updates the kube-config in local for the respective AKS)
* az acr login -n <<ACR-Name>>
 
## Building Docker Image & Pushing in ACR
* docker build -t spring-app:0.7 .
* docker tag spring-app:0.7 acr-name-url/spring-app:0.7
* docker push acr-name0url/spring-app:0.7
