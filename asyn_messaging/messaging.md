# Messaging

### Why Message Queue?
* Listen to Heartbeat of different applications
* Persistence of the data
* Load Balancing

Famous Use Case : Pizza Shop
* Taking multiple orders in async mode
* Giving delivery once pizza made
* Steering orders to different store in case of heavy load

### Different Messaging Types
1. **RMI(Remote Method Invocation)**
* Applications need to know remote application's method
* Extremely tightly coupled
* Synchronous

2. **JMS(Java Messaging Service)**
* Sender & Receiver knows message format and destination to use where message has been delivered
* Guaranteed delivery
* Loose coupling
* Types supported : Synchronous & Asynchronous
* Models supported for JMS Client: Point to Point, Publisher-Subscribe
* Queues are for Point to Point Messaging : Sender -> Queue -> Receiver consumes -> Receiver sends acknowledgement. But there can be more than 1 receiver and sender to write in same queue    
* Topics are for Pub-Sub model : Sender Client -> Publishes to Topic -> Receiver Client subscribes to Topic -> Topic message delivered to Receviver client. Each consumer gets a copy of messages. No acknowledgement
* Way of working : Application server creates server session which stores message in pool. Then server session create JMS Session.
* Types of messages supported :  Message, TextMessage, Bytemessage, OjectMessage, MapMessage
* Famous Examples :IBM MQ, TIBCO
* Famous JMS Providers : Active MQ, Rabbit MQ, JBoss MQ, HornetQ, IBM Websphere MQ
* Components : Publisher, Subscriber, JMS Provider, Queue, Topic
* JMS API Programming Model : ConnectionFactory -> Creates Connection -> Creates Session -> Message Producer/Consumer -> Send/Receive

3. **ActiveMQ**
* Open-source multi-protocol supported message broker 
* Written in Java language

4. **RabbitMQ**
* Open-source multi-protocol supported messaged broker
* Written in Erlang language
* Protocols supported : AMQP(Started from here), MQTT, and STOMP
* Point to Point : Sender -> Queue -> Receiver
* Pub-Sub : Sender -> Exchange(Based on binding key) -> Queue -> Receiver

5. **Apache Kafka** 
* Streaming platform (Messaging + Distributed storage + Processing of data)
* Pull-type messaging platform where consumers pull the messages from the broker
* Straightforward routing approach that uses a routing key to send messages to a topic.

6. **Event Hub**
* Azure native messaging which helps in publishing events.

7. **IBM MQ/IBM Websphere MQ**
* Follows same approach as specified in JMS religiously(Queues & Topics)


### References
* [JMS](https://www.youtube.com/watch?v=WAkoubQyCwc)
* [IBM MQ](https://www.youtube.com/watch?v=JtQugdDFkMc)
* [JMS & AMQP](https://chatrabazar.wordpress.com/java-basics/jms/)
* [IBM MQ , JMS with Spring Boot](https://www.youtube.com/watch?v=xdeRdlJnfBA)
* [Rabbit MQ](https://www.youtube.com/watch?v=deG25y_r6OY)