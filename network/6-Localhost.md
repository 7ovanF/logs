# Localhost

Definition: **"A [hostname](5-Hostname) that refers to the current computer used to access it."** Src: [Wikipedia: Localhost](https://en.wikipedia.org/wiki/Localhost)

--- 
### Clarification [(Src: freecodecamp.org)](https://www.freecodecamp.org/news/what-is-localhost/)

Any computer can act as host of a service, including yours. 
When the host computer accesses its own hosted service, the traffic is **routed back** internally within the host's [networking stack](). Such connection is called a **loopback** connection. 
A loopback connection is done when connecting to the IP address 127.0.0.1, or aliased by the hostname **localhost.** 

No internet connection is needed to host (albeit only accessible locally) or do a loopback connection.
> the network stack/kernel networking subsystem is still needed though

--- 

As with normal connections, you also need a port. Example: localhost:2220