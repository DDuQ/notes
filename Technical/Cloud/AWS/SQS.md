# Definition

Stands for **Simple [[Queue]] Service**, and is a fully managed queueing service provided by AWS.

# Queue Types

There are 2 options offered:

| Type             | Pros                                              | Cons                                                       |
| ---------------- | ------------------------------------------------- | ---------------------------------------------------------- |
| <br>**Standard** | <br>• Virtually unlimited throughput              | • Messages may be duplicated<br><br>• No predictable order |
| <br>**FIFO**     | • Exactly-once delivery<br><br>• Guaranteed order | <br>• Limited throughput (~3000 msg/s)                     |
