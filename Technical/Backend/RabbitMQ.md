# Definition

Open-source message broker that has a reliable message delivery and support for flexible routing between producers and consumers.
## Mental model
*Producer -> Consumer*
![[RabbitMQ-1.jpeg|545x94]]

*Producer -> Competing consumers*
![[RabbitMQ-2.jpeg|545x208]]

**NOTE:** In practice [[RabbitMQ]] never publishes directly to a [[RabbitMQ#Queue|queue]], it does it through an [[RabbitMQ#Exchange|exchange]].
# Exchange

Core component which routes messages to [[RabbitMQ#Queue|queues]] based on certain rules, called [[RabbitMQ#Bindings|bindings]].

There are different types of exchanges that achieve specific use-cases:
## Direct
- Routes message to queues where the **binding key exactly matches** the message's routing key.
- Useful for point-to-point communication.
  
## Fanout
- Routes messages to **all queues** bound to it, ignoring routing keys.
- Useful for broadcasting messages.

## Topic
- Routes messages based on **pattern matching** (using wildcards like `*` and `#`) against routing keys.
- Useful for pub/sub systems with selective subscribers.

## Headers
- Routes messages based on message **headers** instead of routing keys.
- Useful when routing logic depends on multiple attributes.

# Queue

This is a data structure which (in the context of message brokers) stores messages until they are delivered to and processed by a consumer.

It follows the philosophy of FIFO; *First In First Out*.

**NOTE:** Consumers never directly listen messages from exchanges, they do it through queues. 

# Bindings

Is the link between a [[RabbitMQ#Exchange|exchange]] and a [[RabbitMQ#Queue|queue]], without it exchanges would not be able to identify which messages should go in any queue.
## Binding key
When creating bindings, usually we would specify a **key**, this gets compared against message's [[RabbitMQ#Routing key|routing key]] to decide if a queue gets the message or not (except in [[RabbitMQ#Fanout|fanout exchanges]]).

# Message

Unit of data used by producers / consumers during its communication. It consists of two parts:

1. Payload
	 The actual data to be sent (e.g. JSON, string, binary).

2. Properties / Metadata
	 Headers, content type, delivery mode (persistent or not), timestamp, etc.
	 Routing key is one of these fields.

## Routing key
String field attached to the message by the producer when publishing.
