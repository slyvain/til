
# Best Practices for building Async APIs

:notebook: Notes from video [Best Practices for Building Async APIs with ASP.NET Core](https://www.youtube.com/watch?v=_T3kvAxAPpQ)

## Scalability
The main advantage of Async is not performance, but scalability

**Horizontal scaling**

> What: Adding additional physical or virtual servers.

- writing RESTful system (stateless over http) is a good start to horizontal scaling

- non-distributed DB or cache are an issue

**Vertical scaling**

> What: Increasing the scalability by writing an API in such a way that resource utilization is improved.

- increase the resources of the existing servers (cpu, memory, etc)

- allow for better use of threads and/or free up threads: that's where Async comes into play

## When to use Async

**I/O bound operations**
> Will the code be waiting for a task to complete before continuing?

Typically any call to a database, a file system or a network resource (another API).

:heavy_check_mark: Server-side and client-side communication: **use Async**

**Computational-bound operations**
> Will the code be performing an expensive computation?

:x: Client-side only: **do not use Async, use multi-thread instead**

## Tips

:x: `Task.Result()` does not deadlock anymore in ASP .NET Core but it does block the thread

:warning: `ConfigureAwait(false)` configures a `Task` so that continuation after the `await` does not have to be run in the caller context. This is not necessary anymore in ASP .NET Core as there is no more synchronization context. However it can be useful in a library that targets .Net Standard in order to be used by a .Net Framework project.