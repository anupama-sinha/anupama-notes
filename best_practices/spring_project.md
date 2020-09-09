1. Use Sonarlint Plugin for IDE to minimize code smells.
2. Use Loggers for logging any failures or success messages.(Never use Sysout)
3. Use Starter dependencies and <dependencyManagement> tag to have version uniformity.
4. Bind reponses in ResponseEntity object to get the appropriate HTTP Status Codes.
5. Ensure code modularity for reuse.
6. Have OpenAPI3 integration for exposing endpoints of the Service.
7. Always have spring.application.name for the project
8. Follow Richardson Maturity Model for endpoint nomenclature.
9. Serialize the POJOs and have SerialVersionUID always.
10. Use Lombok Annotations for POJO as far as possible
11. Save all hard-coded values in Properties file
12. Always go for Unchecked Exception which implements RuntimeException
13. Follow proper [Semantic versioning of maven based projects](https://semver.org)
14. Write unit tests and have code coverage report using Jacoco.
15. Replace for loop with [Java 8 Lamda functions](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html).
