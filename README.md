# Multithreading 
Multithreading is a way for a program to handle multiple tasks at the same time. Think of it like a team in a restaurant: while one person cooks, another serves customers, and someone else handles payments. In a computer program, multithreading allows different parts of a program to work simultaneously, improving efficiency and speed.  

Before multithreading, servers used simpler methods to manage tasks. In the single-threaded approach, the server handled one request at a time. If a user made a request, the server had to complete it before moving on to the next. This worked for small systems but became a problem when more users started accessing the server. Another approach was to create a separate process for each request. This allowed multiple requests to be handled at once, but it came with its own issues. Processes are heavy, and creating many of them used a lot of memory and CPU power, making the system slow and resource-intensive.  

As applications grew more complex and user demands increased, it became clear that a better solution was needed. Multithreading solved this by enabling servers to handle many tasks concurrently. Threads are lighter than processes and share memory, which reduces the overhead. This means a server can efficiently process requests like retrieving data from a database, uploading files, or responding to user actions without making others wait. By doing so, servers became faster and more responsive, even when many users interacted with them at the same time.  

In Node.js, which is known for its single-threaded event loop, multithreading can still be achieved using worker threads for heavy tasks. Here is an example of how this works:  

```javascript
const { Worker, isMainThread, parentPort } = require('worker_threads');

if (isMainThread) {
  console.log('Main thread: Starting server.');

  // Create a worker to handle a heavy task
  const worker = new Worker(__filename);
  
  worker.on('message', (message) => {
    console.log('Main thread received:', message);
  });
  
  console.log('Main thread: Doing other tasks...');
} else {
  console.log('Worker thread: Performing a heavy task.');
  
  // Simulate a heavy task
  const result = Array(1e7).reduce((sum, _, i) => sum + i, 0);
  
  parentPort.postMessage(`Worker thread: Task completed with result = ${result}`);
}
```

In this example, the main thread starts and creates a worker thread to handle a computationally heavy task. While the worker thread works, the main thread continues handling other tasks, such as responding to incoming requests. Once the worker completes the task, it sends the result back to the main thread. This ensures that the application does not slow down or become unresponsive while handling multiple tasks.  

Multithreading transformed the way servers work, making them capable of handling the needs of modern applications. It ensures that even with heavy workloads or numerous users, the system remains efficient, responsive, and capable of providing a seamless experience.
