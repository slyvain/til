# Versioning APIs with ASP.NET Core

:notebook: Notes from video [Versioning APIs with ASP.NET Core](https://youtu.be/ryPo5hYHSzM)

## Why and how version an API

The idea is to evolve the API without breaking the clients consuming it.

> Keep in mind to make it easy for the client, not yourself.

There are different ways to do it:

##### 1. in the URI
```
https://foo.org/api/v2/Customers
```

- more explorable
- make sense with broad changes, but all clients must update their URIs
- does not allow default/fallback version

##### 2. with a Query String
```
https://foo.org/api/Customers?v=2.0
```

- allow implicit default fallback version

##### 3. with Headers
```
Content-Type: application/json
X-Version: 2.0
```

- clients need to know how to edit the headers

##### 4. with Accept Header
```
Content-Type: application/json
Accept: application/json;version=2.0
```

##### 5. with Content Type
```
Content-Type: application/vnd.yourapp.camp.v1+json
Accept: application/vnd.yourapp.camp.v1+json
```

- bulletproof: in case the API changes between when you get the data and send back
- most complex solution

## Versioning in ASP.NET Core

##### 1. Enable versioning

- Install NuGet package `Microsfot.AspNetCore.Mvc.Versioning`
- In `Startup.ConfigureServices()` method, add `services.AddApiVersioning();`
- Configure API versioning:
```csharp
services.AddApiVersioning(options => {
  options.DefaultApiVersion = new ApiVersion(1,0);
  options.AssumeDefaultVersionWhenUnspecified = true;
  options.ReportApiVersions = true; // Indicates supported version(s) in the response headers
});
```

##### 2. Version controllers

Decorate the controller with an attribute for each supported version:
```csharp
[ApiVersion("1.0")]
[ApiVersion("1.1")]
[ApiVersion("2.0")]
public class MyController
```

##### 3. Version endpoints

Each endpoint can the be mapped to a version like so:
```csharp
[MapToApiVersion("1.0")]
public IActionResult Get(int Id)

[MapToApiVersion("1.1")]
public IActionResult Get11(int Id) // Rename method to prevent compile error
```

##### 4. Enable different ways of versioning

The default way to give the requested version
```
/api/customers/1?api-version=1.0
```
can be changed in the configuration:

```csharp
options.ApiVersionReader = new HeaderApiVersionReader("X-Version");
```
which now allow setting the version in the custom headers.

More possibilities by combining VersionReader elements:
```csharp
options.ApiVersionReader = ApiVersionReader.Combine(
  new HeaderApiVersionReader("X-Version"), // Use the name you want
  new QueryStringApiVersionreader("v")); // Use the name you want
```
In case the use indicates two different versions, it will throw an `AmbiguousApiVersion` exception.


:pencil: TODO: check how it shows in swagger