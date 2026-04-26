# Dependency Injection

Instead of creating a Service object itself, let the Client class receive its Service from an Injector.

Terminology: Client is the receiver of the service, Injector injects the service to Client.

Benefits:
- You can use that component multiple times across multiple dependent Clients in one go.
- 
- Testing: you can inject mock or stub dependencies when testing. 
  >Stub and mock: go read about it...