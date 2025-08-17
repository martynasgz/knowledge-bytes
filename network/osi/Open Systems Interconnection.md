# Open Systems Interconnection

> Credit to https://youtu.be/gYN2qN11-wE

## Terms

**Host** - any device which sends or receives traffic. Hosts are identified by their **IP address**.

**Network** - logical grouping of hosts which require similar connectivity. Networks can contain other networks, called subnets:
![networks and subnets](/assets/2025-08-15-19-14-36.png)

Hosts on the network share the same space IP address space:
![network example](/assets/2025-08-15-19-46-10.png)

The reason one might separate sets of networks is because they need different connectivity requirements. But, in the end, if one wants those networks to communicate between each other, they'll need a [[3. Network#Router|router]].

## Layers

1. [[1. Physical|Physical]]
2. [[2. Data Link|Data Link]]
3. [[3. Network|Network]]
4. [[4. Transport|Transport]]

## OSI vs. TCP/IP

When the OSI model was first created, each of the the last 3 layers (session, presentation & application) had a distinct function, independent from the rest. But recently the distinction has become pretty vague - every application is free to implement the function of these 3 layers as they choose. Therefore, often, these layers are considered as a single application layer, which is exactly what the TCP/IP model does.

## Communication Between Hosts

The sending host's first step will always be to determine whether the target is in the local or foreign network. Depending on that it's going to choose what to make the [[Address Resolution Protocol|ARP]] request for:
- Foreign - ARP for default gateway's IP.
- Local - ARP for the target IP.

### Same Network

Let's say, two hosts are directly connected to each other:
> The shortened MAC, IP address & subnet mask are below each host.
```
[data]->
[A]-----------------[B]
a2a2                b2b3
10.1.1.22           10.1.1.3
255.255.255.0       255.255.255.0
```
> Subnet masks identify the size of the network!

What host A knows:
- It has some data to send to B.
- Knows the IP address of B because:
	- It was typed with `ping 10.1.1.3`.
	- It was acquired from a DNS.
	- ...
- That host B is in the same network by looking at its IP and subnet (TODO: subnetting at https://subnetipv4.com/).

What host A doesn't know:
- B's MAC address - needs to find it out by using [[Address Resolution Protocol|ARP]].

After finding out the MAC address, host A will:
- Attach a layer 2 & 3 headers.
- Send the data.

### Foreign Network

> /24 is a subnet mask 255.255.255.0.
```
[data]->
[A]-------------{R}-------------[C]
a2a2            e5e5            c4c4
10.1.1.22       10.1.1.1        10.9.9.44
/24             /24             /24
```
What host A knows:
- It has some data to send to C.
- Knows the IP address of C.
- That host C is in a foreign network by looking at its IP and subnet.

So, host A already knows the layer 3 *end-to-end* header, but still needs to find out the *hop-to-hop* layer 2.
Since the target we want to send data to is on a foreign network, we need to go through a router by making a *hop*. But, at this point A's ARP cache is empty and it doesn't have the layer 2 header required to take the data to the router. So, it makes an [[Address Resolution Protocol|ARP request]].
But, how does the host know the router's IP address? It's already configured on its **default gateway**. When a computer is connected to the internet, 3 things have to be configured:
1. IP address.
2. Subnet mask.
3. Default gateway.

This can be seen on Windows through `ipconfig`.
So, when A makes a broadcast request for the MAC address & the router responds, the ARP cache will be:

| IP | MAC |
| -- | -- |
| 10.1.1.1 | e5e5 |

Thus, now that A has both 2 & 3 layer headers, it can make the *hop* to the router.
When the router receives the segment, it's going to remove the L2 header, since it was for the latter *hop* only. Now, the router takes over. It is going to have to find the next layer 2 header, and no matter if it's a router, or the host C itself - the same [[#Communication Between Hosts|defined first step]] will be taken.


## Devices

### Layer Specific

- [[1. Physical#Devices / Technology|Physical]]
- [[2. Data Link#Devices|Data Link]]
- [[3. Network#Devices|Network]]

### Those Considered as Switches/Routers

There are many other network devices:
- Access points
- [[#Firewall|Firewalls]]
- [[#Load Balancer|Load balancers]]
- Virtual switches
- Layer 3 switches
- IDS/IPS
- [[#Proxy Server|Proxies]]
- Virtual Routers

But all of them perform routing and/or switching.

#### Firewall

A firewall can be placed on routers, hosts, or can be its own device. It is the first line of defense in protecting the internal network from outside threats. It functions specifically in layers 2, 3, 4 & 7.  
It blocks packets from entering or leaving the network via:
- Stateless inspection: checks & enforces every packet against pre-defined rules.
- Stateful inspection: examines connection state.

#### VPN Concentrator

A device that connects many remote networks and clients to a central corporate network. Concentrators can function at multiple layers of the OSI model - layer 2, 3 & 7. Similarly to a [[VPN|VPN]], it encrypts outgoing and incoming data and uses security protocols to create safe tunnels, but on a larger scale. It is widely used by large companies with many remote employees.

#### Load Balancer

Distributes requests (workload) among different server instances, in a way that no single one gets overloaded.

#### Proxy Server

Appliance that requests resources on behalf of client machines - it hides and protects it, can filter content, and increase network performance via caching.

## Encapsulation Throughout the Flow

Let's say, one host has an application that generates data which is meant to be sent to the other side:
![initial osi encapsulation](/assets/2025-08-15-22-51-57.png)
When sending, the host is going to go through **encapsulation** process:
```
1. Layer 4:          [TCP|DATA] <-- segment
2. Layer 3:      [IP][--DATA--] <-- packet
3. Layer 2: [MAC][----DATA----] <-- frame
4. Layer 1: 101010101010101010
```
I.e. each layer doesn't know what the data contains (what the layer before put inside).  
When receiving, the host is going to go through the opposite - **de-encapsulation** - process:
```
1. Layer 1: 101010101010101010
2. Layer 2: [MAC][----DATA----] <-- frame
3. Layer 3:      [IP][--DATA--] <-- packet
4. Layer 4:          [TCP|DATA] <-- segment
```
In each de-encapsulation step, the receiver checks whether the destination matches to it - if so, it removes the header and passes it up to the next layer.  
So, network devices, protocols operate at specific layers. Exactly that is shown in the picture below:  
![osi encapsulation](/assets/2025-08-15-23-14-30.png)  
Do keep in mind that these boundaries are not strict. For example, ARP protocol links IP with MAC addresses, meaning it operates on layers 2 & 3.  
The OSI model is not rigid, it's rather a conceptualization of the data flow through Internet.
