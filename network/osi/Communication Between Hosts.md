# Communication Between Hosts

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
