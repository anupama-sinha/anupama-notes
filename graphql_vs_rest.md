### Protocol Transition
|Year|Protocol|
|---|---|
|1981|RPC(Remote Procedure Call)
|1991|CORBA(Common Object Request Broker Architecture)|
|1998|SOAP(Simple Object Access Protocol|
|2000|REST(REpresentation State Transfer)|
|2012|GraphQL(Graph Query Language)|

### GraphQl 
* Query language for APIs 
* Dynamic fulfilling of queries with existing data
* Single endpoint of POST with query 
* Customized data fetching : Reducing the need for unrelevant data as opposed to REST static endpoints sending huge response
* Easier to evolve over time with developer tools 
* Any UI Change is faster addressed to with GraphQL approach

### History
* Developed since 2012 by Lee Brown(Facebook) due to data-fetching needs
* Published(Open sourced) in 2015
* Currently under Linux Foundation umbrella

### REST vs GraphQL Approaches
| REST | GraphQL |
|---|---|
|Considers everything as resources and fetches them|Considers everything as connected graph|
|Requires multiple round trips if data to be fetched from different endpoints|A single POST API call with differnt queries solves the problem|
|Over and under fetching of data|Customized data fetching|
|New endpoint needed for different resource with custom data|No new endpoint needed. Only server needs to handle such requests|
|Good with simple applications|Not good with simple applications as types,queries & resolvers need to be defined|
|Simple error handling|Complex error handling|
|Entry point into data is multiple endpoints|List of fields & Mutation types is the entry point|

### Query Example

* HTTP GET : http://myapi/graphql?query={me{name}}

```
{
  me {
    name
  }
}
```

* HTTP POST

```
{
  "query": "...",
  "operationName": "...",
  "variables": { "myVariable": "someValue", ... }
}
```

* Response

```
{
  "data": { ... },
  "errors": [ ... ]
}
```

### Expectations with GraphQL
* Both frontend and backend needs to know data structure. But only backend will work with database
* HTTP Specification for Caching at network layer not followed as in REST. So, needs Caching of data. Caching enables offline data loading

### Common Use Cases for GraphQL
* Different I/Os(Mobile, Website, Echo, Tab, etc.) needing different set of attributes
* GraphQl Client invoking API Gateway. While API Gateway invoking REST Applications 

### Best Practices to follow with GraphQL Client
* Loading current usable component before the other components which would be needed later for rendering, thereby reducing load time
* Data attribute nomenclature must be unique. No two table columns must have same names to remove amiguity
* Persisted ID & variable caching so as to enable offine loading 
* Storing common data in global store(Eg With Redux Global Store with React.js)
* [Authorizing users](https://graphql.org/learn/authorization/) to access specific data based on user_type in API must be properly defined using access control with GraphQL

### Limitations of GraphQL
* Limited tools eg. API Analytics
* REST endpoint dominates in 3rd party tools which would be needed for project building

### [Apollo Platform](https://www.apollographql.com/docs/tutorial/introduction/)
* Implementation of GraphQl  which transfers data between cloud(Server) to frontend app(Client)
* Popular choice of using GraphQL in Javascript based apps

### Conclusion
* Meticulous analysis of application needs to be done to decide which one to go ahead with REST or GraphQL
* Play around the GraphQL playgrounds available online and finalize which one to proceed with

### References
* https://graphql.org/code/
* https://medium.com/@back4apps/graphql-vs-rest-62a3d6c2021d#:~:text=With%20REST%2C%20the%20server%20determines,has%20no%20automatic%20caching%20system.
* [Best Practices for GraphQL](https://youtu.be/1Fg_QtzI7SU)
* [Access Control with GraphQL](https://www.apollographql.com/blog/authorization-in-graphql-452b1c402a9/)
* [Access Control Implementation Example](https://www.pingidentity.com/en/company/blog/posts/2020/graphql-access-control-part-1.html)
* [Authorization ways with Apollo](https://jkettmann.com/3-ways-for-authorization-with-graphql-and-apollo/)