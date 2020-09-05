# Messaging using RabbitMQ

### Definition
* Open-source multi-protocol supported messaged broker
* Written in Erlang language
* Protocols supported : AMQP(Started from here), MQTT, and STOMP

### Points to Note
* Ready messages : Waiting to be consumed by Consumer
* Load Balancing mechanism : Round robin Scheduling
* Message types supported : String, JSON, XML, Serializable Object, etc.
* Ways of Purging Messages
	1. UI : Purge Message
	2. CMD : rabbitmqctl.bat purge_queue <Queue-name>
* There is no max Queue length but is configurable. Refer [Queue Length Page](https://www.rabbitmq.com/maxlength.html)
* Maximum message size is 512MiB from Version 3.8 onwards. Refer [Github Page](https://github.com/rabbitmq/rabbitmq-common/blob/master/include/rabbit.hrl#L250) for entire properties.

### Best Practices for RabbitMQ
Refer [Linked Page](https://www.linkedin.com/pulse/13-rabbitmq-facts-i-wish-knew-from-start-gideon-arom) directly.

### Downloads & Setup

* [RabbitMQ Download](https://www.rabbitmq.com/install-windows-manual.html)
* [ERLang OPT Download](https://www.erlang.org/downloads)
* Set environment variables (ERLANG_HOME : C/PrgFiles/erlang/, Path(Append at end) : %ERLANG_HOME%\bin)
* Enable RabbitMQ Management Plugin from CommandLine (rabbitmq_server-3.8.8\sbin>rabbitmq-plugins.bat enable rabbitmq_management)
* Start the Server : Double click rabbitmq-server.bat(Takes some time to start server)
* Visit the Localhost Installation of RabbitMQ at http://localhost:15672
(Default Credentials : guest/guest)

### Message Flow
* Producer Application writes to Exchange
* Exchange has Conditions of Message Routing
* Exchange routes messages to appropriate Queues
* Consumer Applications listen to the appropriate Quees

### Exchange Types(Based on Binding with Queue)
1. Direct - queue.key = exchange.binding.key 
2. Fanout - Exchange publishes to all bound queues(No Key requirement)
3. Topic - queue.key=Partial key of Exchange.binding. Eg.(Exchange Keys - pen.pencil.eraser)=Queues(*.pen.*,*.pencil.*,#.eraser)[*-Exactly one word, #->Any number of words]
4. Headers - Headers of Exchange = Header of Queue(any/all)

### Dependencies

* RabbitMQ Java client library : Allows Java applications to interface with RabbitMQ.

```xml
<dependency>
	<groupId>com.rabbitmq</groupId>
	<artifactId>amqp-client</artifactId>
	<version>5.6.0</version>
</dependency>
```

* RabbitMQ Dependency for AMQP

```xml
<dependency>
	<groupId>org.springframework.amqp</groupId>
	<artifactId>spring-rabbit</artifactId>
</dependency>
```
### Publishing Messages to Queue

```java
public class Publisher {
	public static void main(String[] args) {
		ConnectionFactory factory = new ConnectionFactory();
		Connection connection = factory.newConnection();
		Channel channel = connection.createChannel();

		// Default Exchange type is Direct
		String exchangeType = "";

		String message = "Hello Anupama!!!";

		// channel.basicPublish(<exchange-name>, <routing-key>,<basic-prop(Eg.Headers)>, <message-body>)
		switch (exchangeType) {
		case "topic":
			channel.basicPublish("Topic-Exchange", "pen.pencil.eraser", null, message.getBytes());
			break;
		case "fanout":
			channel.basicPublish("Fanout-Exchange", "", null, message.getBytes());
			break;
		case "header":
			Map<String, Object> headersMap = new HashMap<String, Object>();
			headersMap.put("item1", "pen");
			headersMap.put("item2", "pencil");
			headersMap.put("item2", "eraser");

			BasicProperties br = new BasicProperties();
			br = br.builder().headers(headersMap).build();
			channel.basicPublish("Headers-Exchange", "", br, message.getBytes());
			break;
		default:
			channel.basicPublish("Direct-Exchange", "pen", null, message.getBytes());
			break;

		}
		channel.close();
		connection.close();
	}
}
```

### Consuming Messages from Queue

```java
public class Consumer {
	public static void main(String[] args) {
		ConnectionFactory factory = new ConnectionFactory();
		Connection connection = factory.newConnection();
		Channel channel = connection.createChannel();
		
		DeliverCallback deliverCallback = (consumerTag, delivery) -> {
			String message = new String (delivery.getBody());
		};
		//channel.basicConsume(<queue-name>, <autoAck-flag>, <deliverCallback> , <cancelCallback>)
		channel.basicConsume("Queue-1", true, deliverCallback, consumerTag -> {});
	}
}
```
### Spring Boot Properties

* spring.rabbitmq.host=localhost
* spring.rabbitmq.port=5672

### Publishing Serializable Custom Object to Queue from Controller

```java
@RestController
@RequestMapping("/publish")
public class RabbitMQPublishingController {
	
	@Autowired
	RabbitTemplate rabbitTemplate;
	
	@Autowired
	Product product;
	
	@GetMapping("/product/{name}")
	public String publishProduct(@PathVariable("name") String name) {
		product.setProductName(name);
		
		//Simple Message Converter used here - Converts String,ByteArray & Serializable object only
		rabbitTemplate.convertAndSend(<exchange-name>,<routing-key>, product);
		
		return "Message Sent Successfully";
	}
}
```

### Consuming Serializable Custom Object from Queue from Service Layer

```java
@Service
public class RabbitMQConsumerService {

	@RabbitListener(queues = "Pens")
	public void getMessage(Product product) {
		//Business Logic using Product Object
	}
}
```

Similarly, serialize the messages as per requirement and publish it. And then de-serialize at Consumer end accordingly. Will come up with a detailed separate post on Serialization concepts. That's it for now. Enjoy learning!! :pray:

### P.S.- Below are some good sites.
* https://blog.theodo.com/2019/08/event-driven-architectures-rabbitmq/
* https://medium.com/swlh/a-quick-guide-to-understanding-rabbitmq-amqp-ba25fdfe421d
