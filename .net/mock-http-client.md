# Mock HttpClient

When running unit tests, we may need to mock an `HttpClient` call. As it's not an integration nor an acceptance test, the call does should not reach a live endpoint and should respond with a predefined response.

`HttpClient` does not offer an interface to mock it easily with Moq or NSubstitute.
However we can leverage the `HttpMessageHandler` which is an abstract class and can be given to the contructor of `HttpClient`.

Here's the pretty self-explanatory code of such an implementation:

```csharp
public static class MockHttpClient
{
    public static HttpClient GetClient(HttpStatusCode responseStatusCode, string responseContent = "")
    {
        var messageHandler = new MockHttpMessageHandler(new HttpResponseMessage
        {
            StatusCode = responseStatusCode,
            Content = new StringContent(responseContent, Encoding.UTF8, "application/json")
        });

        return new HttpClient(messageHandler)
        {
            BaseAddress = new Uri("https://service.uri")
        };
    }

    private sealed class MockHttpMessageHandler : HttpMessageHandler
    {
        private readonly HttpResponseMessage _response;

        public MockHttpMessageHandler(HttpResponseMessage responseMessage)
        {
            _response = responseMessage;
        }

        protected override async Task<HttpResponseMessage> SendAsync(HttpRequestMessage request,
                                                                     CancellationToken cancellationToken)
        {
            return await Task.FromResult(_response);
        }
    }
}
```

And it can be used in unit tests as follow:

```csharp
[Fact]
public async Task TestClient()
{
    var content = "some content";

    var mockHttpClient = MockHttpClient.GetClient(HttpStatusCode.OK, content); // an object can be also be serialized here

    var response = await mockHttpClient.GetAsync("/endpoint");

    var jsonResponse = await response.Content.ReadAsStringAsync();

    Assert.Equal(content, jsonResponse);
}
```