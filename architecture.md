## Backend Server Architecture
* Cloud Option : Azure
* Backend Services(Service Oriented Architecture(SOA)/Monolith) : Retail Service(Add to Cart, Payment,Wishlist, etc) – All Services hosted here


## Problems of SOA/Monolith Application 
1. Hige Application
2. Difficult troubleshooting
3. Development Difficult : All developers need to know entire modules of architecture in order to develop
4. Huge Seasonal Traffic – Increase in instances increases all modules. But not equally distributed traffic for each module. A few module has more traffic than the other. Eg. Payment Service module invoked less than Add to Cart Module
5. Wastage of Memory/OS/Cost

## Frontend Service Architecture of SOA/Monolith Application 
* Entire Javascript Application running in same Server Box
* Huge Load Time of Application due to huge size of bundle.js
* Entire application loaded and rendered on each event

## Backend Miroservices Architecture
* All services broken into smaller modules. Eg. Cart Service , Payment Service, etc

### Types in Microservices Architecture
* Types can be Server Side Architecture(Azure APIM) or Client Side Architecture(Eureka Server,Eureka Client, Neflix Ribbon for Load Balancing, Open Fiegn, Service Registraion & Discovery, Sleuth, Hystrix & Circuit Breaker for error scenario handling)
* API Gateway : Accepts outside network traffic. Usually performs SSL/TLS termination. And starts a new SSL/TLS for inside traffic. Majorly works with HTTP(s) protocol
* While Backend Services are small applications which usually just perfroms the business logic
* Mostly, all Authentication/Authorization and routing are performed by API Gateway
* API Gateway can directly access Backend Services/Frontend Services or else might have Queues in between to properly route the messages/data
* In case of erroneous messages in Queue or any other issues, we will have DLQ(Dead Letter Queue) to store the messages until it is acted upon. There might be acknowledgement concept sent by Consumer to Publisher or there might be Timeout for messages in Queue.
* Load Balancer/Number of Instances for any application is usually configured based on traffic or Server loading. Say if Backend Cart Service is deployed in Tomcat Server, then it can spin max 10K threads. Hence, able to entertain only 10K customers at a time. So, in order to entertain 40K customers, we need to have 4 instances of Cart Service application. Front end never knows which instances it invokes. It just knows that it is invoking Backend Service
* Hystrix/Cicuit Break helps in routing error scenario
* Sleuth/Fishtagging is used to log the messages for Backend Service for later support
* Backend HTTP Responses are in any format(JSON/XML,etc). While there are majorly 3 categories of HTTP Status Codes(2XX : Success scenario, 4XX: Validation Errors, 5XX: Server Error). Usually, in the UI, 5XX is not preferred. Internally the architecture must be robust enough to handle 5XX scenario and give only 2XX or max 4XX responses. A usecase is say End User Customer bought a dress from any Retail UI Portal. While making the Payment, the Third Party Payment Gateway API crashed after the Customer made payment. In that case, we would never tell customer that the payment crashed and refund him the amount. Instead, we will show 2XX(Success) and internally have some buffered Queue in order to reprocess the Payment already made by Customer. Here, based on legal grounds, we need to check the procedures based on which Service it is

## Latest Frontend Architecture
* Need of SPA(Single Page Application) & segregation into smaller Components to have minimized load time threshold value for better User Experience
* Virtual DOM in React.js
* Change Detection Rendering Concept for entire DOM in Angular
* Better routing

## Sample Architecture Diagram
![Architecture](https://user-images.githubusercontent.com/68496768/107845275-56306680-6e00-11eb-87d8-d6cc96a40d68.png)
