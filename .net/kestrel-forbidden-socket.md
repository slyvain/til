# Forbidden access to Kestrel socket

I recently encountered this error out of the blue when starting my project:

> Unable to bind to http://localhost:50505 on the IPv4 loopback interface: 'An attempt was made to access a socket in a way forbidden by its access permissions'.

The culprit is a Windows 10 update.
Windows reserves some ports on the machine, and after the update, the port `50505` was not accessible anymore.

The port ranges that are reserved by Windows can be checked with this command:

`netsh interface ipv4 show excludedportrange protocol=tcp`

We can see below that `50505` is in the excluded range `50420` to `50519`.

![Show excluded port ranges](../assets/kestrel-forbidden-socket.png?raw=true)