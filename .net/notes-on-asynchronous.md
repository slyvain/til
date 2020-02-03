Asynchronous to offload a thread (and not block the UI thread for example), or for scalability.

# Sync-over-async
Sync-over-async happens when making an async call but not awaiting it by calling .Result or .Wait() on the returned task
This causes ThreadPool starvation.
