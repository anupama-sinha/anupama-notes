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
* Models supported : Point to Point, Publisher-Subscribe
* Way of working : Application server creates server session which stores message in pool. Then server session create JMS Session.
* Types of messages supported :  Message, TextMessage, Bytemessage, OjectMessage, MapMessage
* Famous Examples :IBM MQ, TIBCO
	
3. **ActiveMQ**
* Open-source multi-protocol supported message broker 
* Written in Java language

4. **RabbitMQ**
* Open-source multi-protocol supported messaged broker
* Written in Erlang language
* Protocols supported : AMQP(Started from here), MQTT, and STOMP

5. **Apache Kafka** 
* Streaming platform (Messaging + Distributed storage + Processing of data)
* Pull-type messaging platform where consumers pull the messages from the broker
* Straightforward routing approach that uses a routing key to send messages to a topic.

6. **Event Hub**
* Azure native messaging which helps in publishing events.