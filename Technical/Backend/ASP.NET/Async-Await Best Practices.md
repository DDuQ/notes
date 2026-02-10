# `Task.Run` for CPU-Bound Operations

Use `Task.Run` to move CPU-bound code off the main thread and onto a thread pool thread. This prevents the UI from freezing and keeps the application responsive.

**Use cases:**
- Heavy computations (e.g., complex calculations, data processing).
- File I/O operations that are not inherently asynchronous.
- Any operation that consumes significant CPU resources and doesn't have a readily available asynchronous counterpart.

# Use `ConfigureAwait(false)` on Library Code

When writing library code that doesn't need to resume on the original synchronization context, use `ConfigureAwait(false)`. This avoids unnecessary context switching and potential deadlocks, especially in scenarios like ASP.NET.

**Use cases:**
- Writing reusable libraries intended for wide consumption.
- Methods that perform operations that don't depend on UI elements or thread-local state.
- Asynchronous methods called from web applications or services where the original context is not relevant.

# Consider `ValueTask` for Performance-Critical Code

`ValueTask<T>` is a struct that can improve performance by avoiding heap allocations in scenarios where the asynchronous operation completes synchronously. It's a good choice when you expect the operation to finish quickly most of the time.

**Use cases:**
- High-frequency, short-lived asynchronous operations.
- Performance-sensitive code paths where allocation overhead is a concern.
- Scenarios where the result is often available immediately, avoiding the overhead of a `Task` object.

# Wrap `IEnumerable<T>` in `IAsyncEnumerable<T>`

If you have a method that returns an `IEnumerable<T>` and you want to introduce asynchronous operations or streaming capabilities, consider transforming it into an `IAsyncEnumerable<T>` using methods like `ToAsyncEnumerable()`. This allows for asynchronous iteration using `await foreach`.

**Use cases:**
- Reading large files line by line asynchronously.
- Processing data streams where each item might involve an asynchronous operation.
- Implementing asynchronous data sources that can be consumed incrementally.

# Use `Task.WhenAll` to Aggregate Results from Multiple Tasks

`Task.WhenAll` takes an array or collection of tasks and returns a single task that completes when all the input tasks have completed. It's useful for running multiple operations concurrently and then processing their results together.

**Use cases:**
- Fetching data from multiple external APIs simultaneously.
- Performing several independent I/O operations in parallel.
- Running multiple background processes that need to finish before proceeding.

# Avoid `async void` except for Event Handlers

`async void` methods cannot be awaited, making it difficult to handle exceptions properly. The only acceptable use case for `async void` is for event handlers, where the framework typically manages exception handling.

**Use cases:**
- Button click event handlers in UI applications.
- Other event handlers where the signature is pre-defined as `void`.
- **Not recommended for general-purpose asynchronous methods.**