# Debug Json Deserialization

:notebook: Newtonsoft documentation: https://www.newtonsoft.com/json/help/html/SerializationTracing.htm

How to output the deserialization (serialization) process in the Debug window:

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Serialization;
using System.Diagnostics;

ITraceWriter traceWriter = new MemoryTraceWriter();
var settings = new JsonSerializerSettings()
{
    TraceWriter = traceWriter
};

var result = JsonConvert.DeserializeObject<MyObject>("json", settings)

Debug.WriteLine(traceWriter);
```