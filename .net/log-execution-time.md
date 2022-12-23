# Log Execution Time

A simple class to log the execution time of a piece of code.

## Implementation

```csharp
using Microsoft.Extensions.Logging;
using System;
using System.Diagnostics;

namespace Logger;

public class StopwatchLogger : IDisposable
{
    private readonly Stopwatch _sw = Stopwatch.StartNew();
    private readonly ILogger _logger;
    private readonly string _message;

    public StopwatchLogger(ILogger logger, string message)
    {
        _logger = logger;
        _message = message;
    }

    public void Dispose()
    {
        _sw.Stop();
        _logger.LogInformation("{message}: Elapsed Milliseconds: {elapsedMilliseconds}", _message, _sw.ElapsedMilliseconds);
    }
}
```

## Usage:

```csharp
using (new StopwatchLogger(_logger, "Message about profiling"))
{
    // piece of code to profile
}
```