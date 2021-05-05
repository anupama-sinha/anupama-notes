## Azure Key Vault
* Tool to securely store & acess secrets like API Keys, Passwords, Certificates, etc

## Steps to externalize Azure Key Vault in Azure
* Create Service Principal
* Create Key Vault in Azure
* Check [Azure Official Documentation](https://docs.microsoft.com/en-us/azure/developer/java/spring-framework/configure-spring-boot-starter-java-app-with-azure-key-vault

## Spring Boot Integration
* pom.xml

```xml
<properties>
  <azure.version>2.3.5</azure.version>
</properties>  
  
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
    <version>2.3.5</version>
    <scope>runtime</scope>
</dependency>
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-spring-boot-bom</artifactId>
      <version>${azure.version}</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
 </dependencies>
</dependencyManagement>
```

* application.properties

```properties
azure.keyvault.client-id=<app-id from Service Principal>
azure.keyvault.client-key=<Pwd generated during Service Principal Creation>
azure.keyvault.enabled=true
azure.keyvault.tenant-id=<tenant-id for Service Principal>
azure.keyvault.uri=https://xxxxx-keyvault85.vault.azure.net/
```

## Property Usage

* Here connectionString is saved in Azure Key Vault

Option # 1

```java
@Value("${connectionString}")
private String connectionString;
```

Option # 2

```java
anupama.sinha.com.db.connectionString=${connectionString}

@Value("${anupama.sinha.com.db.connectionString}")
private String connectionString;
```
