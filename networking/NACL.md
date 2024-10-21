# Network Access Control Lists (NACL)

- Every subnet has an associated network ACL
    - NACLs filter the traffic crossing the subnet boundary INBOUND or OUTBOUND 
- If you had two EC2s in the same subnet, they would not be affected by network ACLs

Each NACL has two sets of rules: Inbound rules and Outbound Rules. 

```
Rule Number         Type            Protocol        Port Range      Source          Allow/Deny 
100                 All Traffic     All             All             0.0.0.0/0       Allow
*                   All Traffic     All             All             0.0.0.0/0       Deny 
```

- Rules match the DST IP/Range, DST Port, and Protocol and ALLOW or DENY based on that match 
- Rules are processed in order, lowest rule number first. Once a match occurs, processing STOPS
- * is an implicit DENY if nothing else matches 
- NACLs are STATELESS, both the REQUEST and RESPONSE parts of every communication need individual rules (1 x INBOUND and 1 x OUTBOUND)

## Default NACL 

A VPC is created with a default NACL:
- Inbound and Outbound Rules have the implicit deny (*) and an ALLOW ALL rule 
- The result - all traffic is allowed, the NACL has no effect

```
Inbound Rules 
Rule Number         Type            Protocol        Port Range      Source          Allow/Deny 
100                 All Traffic     All             All             0.0.0.0/0       Allow
*                   All Traffic     All             All             0.0.0.0/0       Deny 
```

```
Outbound Rules 
Rule Number         Type            Protocol        Port Range      Destination     Allow/Deny 
100                 All Traffic     All             All             0.0.0.0/0       Allow
*                   All Traffic     All             All             0.0.0.0/0       Deny 
```

## Custom NACLs 

Can be created for a specific VPC and are initially associated with NO subnets:
- They only have 1 INBOUND rule: the implicit (*) DENY 

```
Inbound Rules 
Rule Number         Type            Protocol        Port Range      Source          Allow/Deny 
*                   All Traffic     All             All             0.0.0.0/0       Deny 
```

- They only have 1 OUTBOUND rule: the implicit (*) DENY

```
Outbound Rules 
Rule Number         Type            Protocol        Port Range      Destination     Allow/Deny 
*                   All Traffic     All             All             0.0.0.0/0       Deny 
```

If associated with any subnets, all traffic is DENIED.

# Summary 

- STATELESS - REQUEST and RESPONSE seen as different 
- Only impacts data crossing subnet boundary 
- NACLs can EXPLICITLY ALLOW and DENY 
    - Can block specific IPs and ranges 
- IPs/CIDR, Ports and protocols - no logical resources (AWS resources etc.)
- NACLs cannot be assigned to AWS resources - only subnets 
- Use together with Security Groups to add explicit DENY (Bad IPs/Nets)
- Each subnet can have one NACL (default or custom)
- A NACL can be associated with MANY subnets
