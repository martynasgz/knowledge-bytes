# Data Link

## Devices

### Bridge

Bridges sit between hub-connected hosts, and they only have two ports. They also learn which hosts are on each site, so they contain data in each side, without leakage - a problem observed with hubs:
![communication via hubs](/assets/2025-08-15-19-32-18.png)

### Switch

Switches could be called a combination of hubs and bridges. Like hubs, they can accept many devices, and like bridges, they learn which hosts are on each port. They facilitate communication **within** a network. A switch uses MAC addresses to forward data at the data link layer.
![communication via switches](/assets/2025-08-15-19-38-05.png)

### Router

Routers facilitate communication between networks. They provide a traffic control point, i.e. security, filtering, redirecting.  
Routers learn which networks they are attached to. The knowledge of each of these networks, are called **routes**. They're stored in the router's routing table - all networks it knows about. The knowledge is actually represented as IP addresses in each network the router is attached to:
![route example](/assets/2025-08-15-20-04-24.png)
In the example above, the router is attached to networks `172.16.20.x` & `172.16.30.x`. As the priors interface, it is given the IP address `172.16.20.1`. For the network `172.16.30.x`, it's given `172.16.30.254`. These IP addresses are called **gateways** - each host's way out of their local network.



