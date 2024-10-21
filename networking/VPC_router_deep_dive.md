# VPC Router Deep Dive 

- Virtual Router within a VPC 
- Highly available across all AZs in that region 
- Scalable - no performance management required 
- Routes traffic between subnets 
    - From external networks into the VPC 
    - From the VPC into external networks 

VPC router controls our network activity within the VPC 

- Interface in every subnet ... "subnet +1" address (default GW via DHCP option set) 

Control how the VPC Router routes traffic using route tables 

```
========================================================
|VPC                                                    |
|  +--------+        +--------+                         |
|  |Subnet A|--------|  VPC   |--------+Subnet B+       |
|  |(10.16. |        | Router |        |(10.16. |       |
|  |32.0/20)|        |--------|        |48.0/20)|       |
|  +--------+        |   +    |        +--------+       |
|                    |   |    |                         |
|  +--------+        |   |    |        +--------+       |
|  |Subnet C|--------+   |    +--------|Subnet D|       |
|  |(10.16. |            |             |(10.16. |       |
|  |96.0/20)|            |             |112.0/20)       |
|  +--------+            |             +--------+       |
|                        |                              |
| Each Subnet has its    |    VPC Router connects       |
| own private IP block   |    to all subnets            |
========================================================
```

The VPC Router in the above diagram is situated in the VPC and connected to each of the subnets within the VPC on the subnet +1 address.

Every subnet has a VPC Router interface. 
Interfaces use subnet +1 address 

i.e., for 10.16.32.0/20 subnet, the router interface uses 10.16.32.1 

## IP Address Breakdown

Each IP octet (byte) has a total of 256 possible values (0-255). This is because each octet is made up of 8 bits, and 2^8 = 256.

If we have 10.16.48.0/20, this gives us 4096 total addresses.

Calculation:
32 (total bits in IPv4) - 20 (subnet mask) = 12 
2^12 = 4096 

We subtract 2 for network + broadcast addresses 

Example address allocation:

- 10.16.48.0 = Network address 
- 10.16.48.1 = Reserved by AWS for VPC Router 
- 10.16.48.2 = Reserved by AWS for DNS 
- 10.16.48.3 = Reserved by AWS for future use cases 
- 10.16.63.255 = Network broadcast address 

## Route Tables and VPC Router

Route Tables + VPC Router control what's entering and leaving the VPC 

- Every VPC is created with a main route table 
    - Default for every subnet in the VPC 
- Custom route table can be created and associated with subnets in the VPC - Removing the main RT 
- Subnets are associated with one Route Table (main or custom) 

If you associate with custom, main is dropped. If you drop custom, main is reassigned. 

In the vast majority of cases, no changes are made to a VPC route table - left blank. 

Custom routes added to a custom route table and custom route tables explicitly associated with VPC Subnets 
    - Reason being is that if you do disassociate custom route, it's auto-associated with main route table. Bad for security practices. 

Route tables contain routes - most specific routes first 
    - Priority order: most specific route first. /32 is most specific. /0 is least. /32 > /16 in priority and /16 > /0 etc. 

A route table is just a list of routes 

```
Example of AWS console layout 

Destination                 Target                      Status              Propagated  
10.16.0.0/16                local                       Active              No 
2600:1f18:43.../56          local                       Active              No 
0.0.0.0/0                   igw-1a2b3c4d                Active              No
```

The "Status" column indicates whether the route is currently in use (Active) or not.
The "Propagated" column shows whether the route was automatically propagated from a virtual private gateway.

When traffic is leaving the subnet, the VPC router reviews the IP Packet. 
    - A packet has a source address + destination address together with some data 

The VPC router looks at the destination address of all packets leaving the subnet and compares it to the route table in the destination field.

If more than one route matches, higher priority is given to more specific routes (e.g., /32 compared to /16)

The traffic is then forwarded to its destination which is a target inside AWS (e.g., an Internet Gateway)

An alternative is a target of "local" which means its destination is in the VPC. 

All route tables have at least one route to the local route, and this matches the entire VPC CIDR range. 

Local routes can never be changed, are always present + take priority. They are an exception to the previous rule. 

Remember for exam: Route tables are associated with 0 or more subnets 

Can have an empty route table. But a subnet has to have an associated route table. Either main or custom one. 

Route table controls what happens to data as it leaves the subnet or subnets that route table is associated with. 

Local routes are always there and match IPv4 and IPv6 CIDR Ranges 

A default route (0.0.0.0/0 for IPv4 or ::/0 for IPv6) is what happens if nothing else matches. Generally used to direct any unmatched traffic through to a suitable gateway, i.e., Internet Gateway.
