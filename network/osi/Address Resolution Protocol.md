# Address Resolution Protocol

ARP is used by hosts to find out the other devices MAC address, whilst only knowing their IP.

## How it Works?

Let's say, we have these 2 hosts:
```
[A]-----------------[B]
a2a2                b2b3
10.1.1.22           10.1.1.3
255.255.255.0       255.255.255.0
```
When sending a request, host A will only know the IP address of B, as, usually, it's not provided.
Thus, it will send an ARP request. The request asks for the MAC address associated with the provided target IP, so it includes:
- B's IP address `10.1.1.3`.
- Sender A's own IP & MAC addresses

So, we'd have something like this:
```
	If anyone out
	there has the
	IP 10.1.1.3,     ->
	send me your MAC.->
	My IP/MAC is 
	10.1.1.22/a2a2.
[A]---------------------[B]
a2a2                    b2b3
10.1.1.22               10.1.1.3
255.255.255.0           255.255.255.0
```
Since it needs to find out the destination MAC, it sends a **Broadcast request** with a MAC address of `ff:ff:ff:ff:ff:ff`. The latter address is **reserved** to send a packet to everyone on the local network.
ARP mappings are stored in each host's ARP cache.
When the ARP request reaches B, it is able to save A's mapping `10.1.1.22` -> `a2a2`, since it was sent with the request. And since it knows this information, it's able to send back its MAC address back with a **unicast**:
```
	<-I am 10.1.1.3, 
	<-my MAC is b2b3.
[A]------------------[B]
a2a2                 b2b3
10.1.1.22            10.1.1.3
255.255.255.0        255.255.255.0
ARP cache            ARP cache:
10.1.1.22 -> ?       10.1.1.22 -> a2a2
```
With this response, A's able to map `10.1.1.3` to `b2b3` in its ARC cache.
Now, since it has both layer 2 & 3 data, it is able to send the data with appropriate headers.
AND, further data exchange is going to be simple, since both hosts now have each others ARP mapping cached.

During across-network requests, the procedure remains the same, just that the starting host needs to find the MAC address for a router instead. It finds the router's IP through it's default gateway, and sends an ARP request. When it finds out the MAC address, it can then successfully pass over the data to the router. Also, this MAC address will be reused from the ARP cache whenever sending data outside this network.