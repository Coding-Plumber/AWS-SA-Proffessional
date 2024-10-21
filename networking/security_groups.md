# VPC Security Groups (SG)

- STATEFUL - detect response traffic automatically 
- Allowed (IN or OUT) request = allowed response 
- NO EXPLICIT DENY ... only ALLOW or implicit DENY 
    - Can't block specific bad actors 
- Supports IP/CIDR and logical resources 
    - Including other security groups AND ITSELF 
- Attached to ENIs, not instances (even if the UI shows it this way)

## Key Points

- Security Groups are attached to ENIs, even if the user interface shows the security group attaching to an instance. It's actually attaching to the primary interface (ENI) of the instance.
- Security groups in AWS can only allow traffic; they can't explicitly deny it.
- They're stateful, meaning if inbound traffic is allowed, the corresponding outbound traffic is automatically permitted.
- It's impossible to use security groups to block specific "bad actors" or IP addresses.
- Everything not explicitly allowed is implicitly denied.
- For more granular control, including the ability to explicitly deny traffic from specific sources, you'd need to use Network Access Control Lists (NACLs) in conjunction with security groups.

## Example Architecture

```
------------------------------------------------------------------------------------------------
VPC
---------------------------------           ---------------------------------------
subnet-a (private)                          subnet-b (public)  
--------------                              --------------
SG-1            <-----TCP/1337------------- SG-2            <--------------TCP/443------------      Bob(internet user)
EC2(server)                                 EC2(app)
--------------                              --------------
SG-1 Inbound Rules 
Type            Protocol        Port Range          Source      Description(optional)
custom TCP      TCP             1337                SG-2        inbound from App  
SG-2 Inbound Rules 
Type            Protocol        Port Range          Source      Description(optional)
HTTPS           TCP             443                 0.0.0.0/0   inbound HTTPS  
------------------------------------------------------------------------------------------------
```

- Any inbound traffic to SG-1 will allow the source as the security group 2, so any traffic attached to the ENI from SG-2 using port 1337 will be permitted.

## Logical Referencing and Scaling

Logical referencing scales ... any new instances which use the webSG are allowed to communicate with any instances using the APP SG. 
- This means if we were to scale and need more EC2s to handle the application, we could add them to the security group which would then allow them to use the server in SG-1 still.

## Self References

When a SG references itself as a source in its rules, it allows any resource attached to that SG to communicate with any other resource attached to the same SG.

Security Groups offer advantages over using individual IP addresses:

1. IP Address Flexibility:
   - EC2 instance private IPs can change, especially during restarts or replacements.
   - Using Security Groups instead of specific IPs ensures continued access even if IP addresses change.

2. Auto-scaling Compatibility:
   - In auto-scaling groups, you don't know the IP addresses of future instances in advance.
   - Security Groups automatically apply to new instances, maintaining consistent access rules.

3. Simplified Management:
   - No need to update rules when instance IPs change.
   - Adding or removing instances from a Security Group automatically updates access permissions.

Example of self-referencing and logical referencing:

```
SG-2 Inbound Rules 
Type            Protocol        Port Range          Source      Description(optional)
All traffic     All             All                 SG-2        free comms between SG   (This is a self reference) 
Custom TCP      TCP             1337                SG-38       inbound from server  
```

This configuration allows all instances in SG-2 to communicate with each other freely, and permits inbound traffic on port 1337 from any instance associated with SG-38.
