# Stateful vs Stateless Firewalls

## TCP
[TCP Image](../imgs/TCP.png)

Every "Connection" has two parts - REQUEST (initiation) and RESPONSE

## How it actually happens

First, the client picks a temporary ephemeral port usually between 1024 and 65,535 (dependent on operating system).

Once the port is chosen, it initiates a connection to the server on a well-known destination port (e.g., HTTPS tcp/443).

The server then connects back to the source port from tcp/443 and the destination ephemeral port picked by the client.

For example, the client might send 1037 as the port to initiate, so the server would connect to that on the response.

### Example Scenario

If we had a client and a server:
- Client IP: 119.18.36.73
- Server IP: 1.3.3.7

The client will initiate a connection with the server and send:
- SRC IP: 119.18.36.73
- SRC PORT: 13337
- DST IP: 1.3.3.7
- DST Port: 443

The request is inbound from the server perspective but outbound from the client.

Directionality (INBOUND or OUTBOUND) depends on perspective (CLIENT/SERVER).

Each connection consists of a REQUEST and RESPONSE.

## Stateless Firewall

- Means it doesn't understand the state of connections.
- It sees the request connection from client to server and the response from server to client as two individual parts.
- If a client makes a request to the server, that request is inbound to the server. The response is outbound to the client.
- It's possible to have the inverse where the server has to request to another server (e.g., for updates), so the server is then inbound to the update server and the response from the update server is outbound to the server.

Two important points about stateless firewalls:

1. Any servers where they accept connections and where they initiate connections (e.g., web servers that need to accept connections from clients but also need to do software updates) require two rules for each of these, and they need to be the inverse of each other.

2. Get used to thinking that OUTBOUND rules can be both the request and response, and INBOUND can also be the request and response.

Start by determining the direction of the request and then always keep in mind that with stateless firewalls, you're going to need an inverse rule for the response.

- The request component is always going to be to a well-known port like HTTPS (443) or HTTP (80).
- The issue is that the client uses ephemeral ports, which are a large range of random ports. This makes security engineers uneasy, which is where stateful firewalls come in.

## Stateful Firewalls

Stateful firewalls are intelligent enough to identify the REQUEST and RESPONSE components of a connection as being related. They can link the ports and IPs together.

Advantages of stateful firewalls:
- The firewall automatically lets the response back through, so it doesn't need to be set explicitly.
- Only need to allow the request; the response is automatic.
- This eliminates the need to allow the entire ephemeral port range, which is better for security.
