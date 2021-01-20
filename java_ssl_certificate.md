## Need
* Client must trust server for successful SSL connection
* Default Certificate(TrustStore) is cacerts(C/ProgramFiles/Java/JDK/jre/lib/security). This keeps a Keystore of Certificate Authorities(CA) which issue certificates like Verisign,GoDaddy, etc
* If we need to add another new CA for a new Server, either we can append in cacerts or we can create another new one with name as jssecacerts and copy it under JDK Security folder. jssecacerts takes precedence over cacerts

## Exceptions Raised for missing CA addition
> ValidatorException : PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException : unable to find valid ceritification path to requirested target

## Steps to Generate jssecacerts
* Copy [InstallCert.java](https://github.com/escline/InstallCert/blob/master/InstallCert.java) to IDE
* Run the code and choose Option # 1 to use N7 to generate jsscacerts

## Exporting jssecacerts to Self Signed Certificate(SSA)
> keytool -exportcert -alias anupama-sinha.github.io-1 -keystore jssecacerts -storepass {password} -file anupama-sinha.github.io.cer

> keytool -importcert -alias anupama-sinha.github.io -keystore "C/ProgramFiles/Java/JDK/jre/lib/security/cacerts" -storepass {password} -file anupama-sinha.github.io

## Way to Validate
* From the jdk path which has keytool or Java JDK Bin(Added to Env Variable), run below commands and check count of KeyStore in Before File(cacerts) & After File(jsscacerts). There would be another CA addition for the new Server

> keytool -list -keystore jssecacerts

## References
* https://docs.oracle.com/javase/6/docs/technotes/tools/solaris/keytool.html
* [SSL Workflow Diragram](https://www.digicert.com/what-is-an-ssl-certificate)
