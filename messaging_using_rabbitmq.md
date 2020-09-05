# Messaging using RabbitMQ

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

## RabbitMQ in Depth

### Downloads & Setup

* RabbitMQ : https://www.rabbitmq.com/install-windows-manual.html
* ERLang OPT : https://www.erlang.org/downloads
* Set environment variables (ERLANG_HOME : C/PrgFiles/erlang/, Path(Append at end) : %ERLANG_HOME%\bin)
* Enable RabbitMQ Management Plugin from CommandLine (rabbitmq_server-3.8.8\sbin>rabbitmq-plugins.bat enable rabbitmq_management)
* Start the Server : Double click rabbitmq-server.bat(Takes some time to start server)
* Visit the Localhost Installation of RabbitMQ at http://localhost:15672(Default Credentials : guest/guest)

### Message Flow in RabbitMQ
* Producer Application writes to Exchange
* Exchange has Conditions of Message Routing
* Exchange routes messages to appropriate Queues
* Consumer Applications listen to the appropriate Quees

### Exchange Types(Based on Binding with Queue)
1. Direct - queue.key = exchange.binding.key 
2. Fanout - Exchange publishes to all bound queues(No Key requirement)
3. Topic - queue.key=Partial key of Exchange.binding. Eg.(Exchange Keys - pen.pencil.eraser)=Queues(*.pen.*,*.pencil.*,#.eraser)[*-Exactly one word, #->Any number of words]										   
4. Headers - Headers of Exchange = Header of Queue(any/all)

### Points to Note
* Ready messages : Waiting to be consumed by Consumer
* Load Balancing mechanism : Round robin Scheduling
* Ways of Purging Messages
. [UI]  Purge Message
. [CMD] rabbitmqctl.bat purge_queue <Queue-name>

### Dependencies

* RabbitMQ Java client library : Allows Java applications to interface with RabbitMQ.

```xml
<dependency>
	<groupId>com.rabbitmq</groupId>
	<artifactId>amqp-client</artifactId>
	<version>5.6.0</version>
</dependency>
```

### Publishing Messages to Queue

```java
public class Publisher {
	public static void main(String[] args) throws IOException, TimeoutException {
		ConnectionFactory factory = new ConnectionFactory();
		Connection connection = factory.newConnection();
		Channel channel = connection.createChannel();

    //Message Body can be String,Array of String, JSON
  	channel.basicPublish(<exchange-name>, <routing-key>, <basic-prop(Eg.Headers)>, <message-body>);
		
    channel.close();
		connection.close();
	}
}
```

### Consuming Messages from Queue

```java
public class Consumer {
	public static void main(String[] args) throws IOException, TimeoutException {
		ConnectionFactory factory = new ConnectionFactory();
		Connection connection = factory.newConnection();
		Channel channel = connection.createChannel();
		
		DeliverCallback deliverCallback = (consumerTag, delivery) -> {
			String message = new String (delivery.getBody());
		};
		channel.basicConsume(<queue-name>, <autoAck-flag>, <deliverCallback> , <cancelCallback>);
	}
}
```
