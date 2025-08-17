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

### Same Network

Let's say, two hosts are directly connected to each other:
> The shortened MAC, the IP address & subnet mask are below each host.
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
