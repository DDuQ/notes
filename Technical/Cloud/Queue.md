# Definition

This is a data structure which (in the context of message brokers) stores messages until they are delivered to and processed by a consumer.

It follows the philosophy of FIFO; *First In First Out*.

>[!NOTE]
Consumers never directly listen messages from exchanges, they do it through queues. 
