# Definition

Azure Functions is a **[[Serverless|serverless]] compute service** that lets you run small pieces of code (“functions”) in the cloud without managing servers or infrastructure.

> [!INFO]  
> **Cost-effective:** Azure grants **1 million free executions per month**, making it ideal for event-driven workloads.
# Function Triggers

A **trigger** defines _how and when_ a function is executed. Each function can only have **one trigger**, and the type of trigger determines its invocation scenario.
## Http trigger
Executes when the function receives an **HTTP request**.  

>[!INFO]
Useful for REST endpoints, webhooks, or simple APIs.
## Timer trigger
Executes on a **schedule**, defined using a CRON expression.

> "cron is a basic utility available on Unix-based systems. It enables users to schedule tasks to run periodically at a specified date/time."
> Source: [A Guide To Cron Expressions | Baeldung](https://www.baeldung.com/cron-expressions)

## Queue trigger
Executes when a new **message** is placed into an Azure Queue Storage queue.  

>[!INFO]
Ideal for background processing, decoupling services, or handling workloads asynchronously.

# Bindings

Bindings are a way to **connect your Azure Function to external data or services** without writing boilerplate integration code.  

They let you _receive data_ ([[Azure Functions#**Input Bindings**|input bindings]]) or _send data_ ([[Azure Functions#**Output Bindings**|output bindings]]) declaratively through configuration.

A function can have:
- **One trigger**
- **Zero or more input bindings**
- **Zero or more output bindings**

Bindings are defined in the function configuration (e.g., `function.json`) and handled automatically by the Functions runtime.

## **Input Bindings**
Provide data **to** the function.

>[!INFO] Use cases
>- Retrieve an item from **Cosmos DB**
>- Read a blob from **Blob Storage**
>- Fetch a message from **Queue Storage** (in addition to the trigger)

## **Output Bindings**
Allow the function to send data **out** without managing SDKs manually.

>[!INFO] Use cases
>- Write a record to **Cosmos DB**
>- Upload a file to **Blob Storage**
>- Enqueue a new message in **Queue Storage**

# Regular functions VS Durable functions

| Topic                 | Regular Functions                                                               | Durable Functions                                                                |
| --------------------- | :------------------------------------------------------------------------------ | :------------------------------------------------------------------------------- |
| **State Management**  | *Stateless* -- state is not preserved between executions.                       | *Stateful* -- Built-in state management and checkpoints across executions.       |
| **Execution Time**    | Short-lived; best for quick operations (seconds to a few minutes).              | Supports long-running workflows (minutes, hours, days, or longer).               |
| <br>**Complexity**    | <br>Simple to build and maintain.                                               | Higher complexity due to orchestrators, activities, and patterns.                |
| <br>**Orchestration** | No orchestration; each function runs independently unless manually coordinated. | Native orchestration with workflow patterns (fan-out/fan-in, chaining, retries). |
| <br>**Use Cases**     | REST endpoints, queue processing, scheduled jobs, simple event handling.        | Multi-step workflows, approvals, long-running tasks, complex business processes. |
