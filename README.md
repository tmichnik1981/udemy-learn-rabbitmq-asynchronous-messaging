# Learn RabbitMQ: Asynchronous Messaging with Java

#### Getting started

##### Installation

1. Download  [RabbitMQ Windows installer](https://www.rabbitmq.com/install-windows.html)
2. Download [Erland Windows Binary File](https://www.erlang.org/downloads)
3. Install Erlang
4. Install RabbitMQ
   - Allow on connections
   - Install as service
   - Scripts: `c:\Program Files\RabbitMQ Server\rabbitmq_server-3.6.6\sbin\`
5. Go to Services and look for RabbitMQ, verify if it is running

##### RabbitMQ Admin

- Default URL: localhost:15672

- If RabbitMQ Admin is not working you might ned to enable it

  - Go to scripts folder and type

    ```shell
    # on linux/unix
    sh rabbitmq-plugins enable management_plugin
    ```

- Default credentials: **guest/guest**.

###### Management UI

- Overview
- Connections - active connections to a broker
- Channells - messaging channels provided by RabbitMQ
- Exchanges - default exchanges provided for us
- Queues - nothing at start
- Admin - users,  virtual hosts, limits, cluster mechanism, policies

##### Creating Queue, Exchange and Binding etc.

###### Create an Exchange

1. Go to Exchanges
2. Click **Add exchange**
   - Name: "TestExchange"
   - Type: **direct**
   - Durability: **Durable**
   - Auto delete: **No**
   - Internal: **No**
3. Click **Add exchange**
4. Find a new exchange in a table and click on ii

###### Create a Queue

1. Go to Queues
2. Click **Add a new queue**
   - Name: TestQueue
   - Durability: Durable
   - Auto delete: No
3. Click **Add queue**

###### Create a Binding

1. Go to Exchanges
2. Select previously created "TestExchange"
3. Click **Bindings**
   - Select **To queue**
   - Type; "TestQueue"
4. Click **Bind**
5. Click **Publish message**
   - Type some text in **Payload** section
   - Click **Publish message**
6. Go to **Queues** and select TestQueue
7. Click **Get messages**

#### Messaging, AMQP

##### Messaging

- Loosely-coupled integration of software or application components
- Message containes a header and a body

##### Messaging protocols

- STOMP
  - Simple Text Oriented Protocol
  - Can be used anywhere aith a proper client implementation
  - HTTP - like design
  - No concepts like queues or topics. Uses a SEND semantic with a "destination" string for where the message to deliver
  - The receiver can implement queues, topics and exchanges
  - Consumers of messages SUBSCRIBE to these destinations
  - [Home page](https://stomp.github.io/)
- MQTT
  - Message Queue Telemetry Transport
  - Machine to Machine / IoT connectivity protocol
  - Highly-standarized
  - Simply publish-subscribe messaging
  - Designed for resource-constrained devices and low bandwidth, high latency networks such as dial up lines and satellite links
  - Full-featured "enterprise messaging" and very lightweight ideal for mobile and IoT
  - Supports thousands of concurrent device connections
  - Compact binary packets; no message properties, headers compressed
  - AWS IoT and GreenGrass edge-computing solutions are designed around MQTT
- AMQP
  - Advanced Message Queuing Protocol
  - Reliable and interoperable
  - AMQP solutions form different vendors "does" work which is where other protocols failed
  - Highly standarized
  - Provide a wide range of features related to messaging including reliable queuing, topic-based publish-and-suscribe messaging, flexible routing, transactions, and security

###### Usecases for AMQP

- A real time feed of constantly updating information.
- An encrypted assured transaction.
- Want a message to be delivered when the destination comes online.
- Sending an enormous message while still receiving status updated over the same network connection.
- Want things to work on all popular operating systems and languages.

##### RabbitMQ

- OS message broker
- Most popular implementation of AMQP
- A robust and flexible messaging platform designed to interoperate with other messagiong systems
- Developed using Erlang so it boasts on its high throughput and low latency
- Supports clustering for fault tolerance and scalability
- Protocol defines exchanges queues and bindings
- Allows multiple connection channels inside a single TCP conncetion in order to remove the overhead of opening alrge number of TCP connections to the message boker

###### 4 Actors

- **Producer** sends messages to **Exchange**
- A message is routed from an **Exchange** to a **Queue(s)**
- **Consumers** receives messages form a queue

###### Exchanges

- Actual AMQP elements where messages are sent at first
- Takes a message and routes it into one or more queues
- Routing algorithm decides where to send messages from exchange
- Routing algorithms depends on the exchange tyoe and rules called "bindings"
- Bindings are simply used to bind exchanges to queues for messages delivery
- 4 Exchange types: 
  - **Direct Exchange** : (Empty string) and amq.direct. No binding or no configuration.
  - **Fanout Exchange** : amq.fanout. Messages are sent everywhere with its bindings.
  - **Topic Exchange** : amq.topic.  Specific way of delivering messages.
  - **Headers Exchange** : amq.match. Exchanges headers with other exchanges or consumers

###### QUEUES

- Messages are being routed from exchanges to queues
- Queues are final destinations in RabbitMQ before being received by subscribers
- Bindings are simply used to bind exchanges to queues for message delivery.
- Properties of a Queue:
  - **Name** : the name of the queue
  - **Durable** : resist the queue to the disk or not
  - **Exclusive** : delete the queue if not used anymore
  - **Auto-Delete** : delete the queue when consumer unsubscribes

###### Topics

- The "subject" part of the messages
- Defined as **routing_key** for message grouping
- Special formatting for better use:
  - "app.logs.error"
- Optional parameters for message exchange
- You can send and receive messages without any topic information
- Topic Exchanges are defined using Topics for message delivery

###### Bindings

- Rules that exchanges use to route messages to queues
- May have an optional **routing key** attribute used by some exchange types
- **Routing key** acts like a filter
- If message cannot be routed to any queue (there are no bindings) it is either dropped or returned to the publisher, depending on message attributes the publisher has set.