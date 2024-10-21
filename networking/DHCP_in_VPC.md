# DHCP in a VPC

## Dynamic Host Configuration Protocol (DHCP)
- Provides automatic configuration for network resources
- Devices like mobile phones, laptops, and EC2 instances use DHCP to obtain IP addresses
- Process starts with a Layer 2 broadcast to get information from DHCP server
- Information provided includes:
  - IP address
  - Subnet mask
  - Default gateway
  - DNS servers and domain name
  - NTP servers
  - NetBIOS name servers and node type

## AWS DNS Configuration
- AWS provides default DNS (AmazonProvidedDNS)
- Custom DNS can be configured if needed

## DHCP Option Sets in AWS
- Configurations that can be associated with VPCs
- Immutable: cannot be changed once created
- Each VPC can have a maximum of 1 DHCP option set associated with it
- Associating a new option set is immediate, but changes require a DHCP renewal (which takes time to propagate)

## IP Address Allocation in VPCs
- IP addresses are allocated automatically within the VPC
- Static private IP addresses can be assigned to instances if needed
- Subnet mask is based on the VPC subnet configuration
- Default gateway is always the ".1" address of the subnet (e.g., 10.0.0.1 for a 10.0.0.0/24 subnet)

## DNS in VPCs
- Route 53 Resolver is the default DNS service (AmazonProvidedDNS)
- Located at the base of the VPC network range plus two (e.g., 10.0.0.2 for a 10.0.0.0/16 VPC)
- Each subnet reserves the first four IP addresses and the last IP address for AWS use

## Configurable Options in DHCP Option Sets
- DNS servers (default or custom)
- Domain name
- NTP servers
- NetBIOS name servers and node type

## DNS Allocation for VPC Resources
- All resources provisioned in a VPC get DNS names allocated
- Private IP addresses always get a private DNS name
- Public IP addresses get both private and public DNS names (if enabled at the VPC level)

## Note on Public DNS Names
- Public DNS hostname allocation can be enabled or disabled at the VPC level
- When enabled, instances with public IPs receive public DNS names automatically

## Important VPC Networking Concepts
- VPC router: handles traffic between subnets and to/from internet gateway
- Internet Gateway: allows communication between VPC and the internet
- NAT Gateway: allows private subnet resources to access the internet
- VPC Endpoints: enable private connections to supported AWS services

## Best Practices
- Use DHCP option sets to ensure consistent network configuration across your VPC
- Consider using custom DNS servers for advanced networking scenarios
- Regularly review and update DHCP option sets as part of your network management strategy
