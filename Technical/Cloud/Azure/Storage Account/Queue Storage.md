# Definition

Azure [[Queue]] Storage is a cloud service ==designed to store large numbers of messages== that can be processed asynchronously.

## Use cases

- When two parts of a system need to communicate **without being online at the same time**.
- For **background jobs**, task scheduling, and event-driven processing.
- To **decouple systems** (e.g., your API puts a message in the queue, a worker processes it later).