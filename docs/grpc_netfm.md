# Build gRPC client with .Net Framework 4.8 by using gRPC C# core

ASP.Net Core gRPC is handy for building service, but if the client is built on .Net Framework, you need to have some extra configuration on the client side to access the service.

There's an article from Microsoft to explain this: [Use gRPC client with .NET Standard 2.0](https://docs.microsoft.com/en-us/aspnet/core/grpc/netstandard?view=aspnetcore-5.0#net-framework)

So according to the article, for client that are built on .Net Framework, either you use WinHttpHandler or gRPC C# core-library. The solution with WinHttpHandler is simple but with some limitations.

The following is to discuss how to use [gRPC C# core-library](https://grpc.io/docs/languages/csharp/quickstart/) to realize the goal.

### Setup Reference
Add the following reference to client via NuGet

* Grpc.Core
* Grpc.Core.Api
* Google.Protobuf

### Handle SSL credential
Assuming the server side is secured via SSL, so you need to setup a SslCredentials when initialize the channel. To so this, you need to extract the certificate (Assuming it's a **pfx** format), run following command:

```bash
openssl pkcs12 -in "YOUR.pfx" -nokeys -out "YOUR.pem"
```
It will generate a PEM file including the encoded certificate. You will need the pem file to generate the credential.

### Setup the client
In the .Net Framework code, initialize the channel and client as following:
```c#
   var credentials = new SslCredentials(File.ReadAllText("YOUR.pem"));
   var channel = new Channel("SERVER_HOST_NAME", SERVER_PORT_NUMBER, credentials);
   var client = new YourGrpcServiceClient(channel);
```
Be careful when you use gRPC C# core to initialize the Channel, you just need to specify the server host name, without **https://**

So now you should be able to run the client and access your gRpc service on the server.






 
